> ℹ️ **Nota:** Este documento es la versión en español de `technical-justification-IPVC.md`.
> La versión en inglés se mantiene como documento oficial para el envío a OWASP.  

# Justificación Técnica para OWASP  
## Título: Propuesta independiente para validación de integridad en pipelines — IPVC  
## Por qué IPVC debe existir como proyecto OWASP independiente  

**Autor y Project Leader propuesto:** Fernando Flores Alvarado  
**Correo OWASP:** fernando.alvarado@owasp.org  
**Fecha:** Junio 2026  
**Tipo de proyecto:** Documentación + Herramienta  
**Licencia:** Apache 2.0 (código) + CC BY 4.0 (documentación)  
**Proyecto OWASP relacionado:** [RHC — Randomized Header Channel for CSRF Protection](https://github.com/OWASP/www-project-randomized-header-channel-for-csrf-protection)  
**Estado:** Pendiente de envío formal  

---

## Contexto  

Este documento responde directamente a la pregunta que OWASP naturalmente hace al evaluar una nueva propuesta de proyecto:  

> *¿Por qué este proyecto debe existir de forma independiente de los recursos OWASP existentes y de marcos establecidos como SLSA y Sigstore?*  

Esta propuesta no tiene la intención de reemplazar ningún recurso OWASP existente, ni de modificar el contenido de la CI/CD Security Cheat Sheet. En cambio, introduce una nueva capa arquitectónica diseñada específicamente para un problema que los marcos actuales no abordan formalmente.  

---

## La brecha específica  

Los pipelines CI/CD modernos son flujos automatizados que transforman código fuente en software desplegado. Este proceso pasa por múltiples etapas secuenciales: revisión de código, análisis de dependencias, build, pruebas, escaneo de seguridad y despliegue.  

**El problema central es que ningún mecanismo formal valida actualmente la integridad del resultado de cada etapa antes de que el proceso continúe.**  

Las siguientes preguntas quedan sin respuesta en cada transición del pipeline por parte de cualquier marco existente:  

- ¿El proceso que se ejecutó fue exactamente el que se esperaba?
- ¿El resultado producido coincide con lo que se declaró previamente?
- ¿El flujo fue alterado entre etapas?
- ¿La integridad del artifact se mantuvo desde la etapa anterior?

Esta brecha no es teórica. En mayo de 2026, Andrew van der Stock, Director Ejecutivo de OWASP, alertó a la comunidad de líderes sobre un incremento significativo de ataques a la cadena de suministro mediante Pull Requests maliciosos, bots de IA y código ofuscado en proyectos OWASP. El vector de ataque explota precisamente esta brecha: la ausencia de validación formal entre etapas del pipeline.  

---

## Por qué los marcos existentes no cubren esta capa  

### SLSA (Supply-chain Levels for Software Artifacts)  

SLSA define niveles de confianza para el proceso de build y el artifact resultante. Responde a la pregunta: *"¿Qué tan confiable es este artifact y cómo fue construido?"*  

SLSA no aborda lo que ocurre entre etapas durante la ejecución del pipeline. No valida que la transición de revisión a build, o de build a escaneo, o de escaneo a despliegue, haya producido exactamente el resultado esperado. Un atacante que introduce código malicioso después de la etapa de build — por ejemplo, manipulando una GitHub Action que corre durante la etapa de escaneo — está fuera del alcance de SLSA.  

### Sigstore  

Sigstore provee firmas criptográficas para artifacts con el fin de garantizar su autenticidad y origen. Responde a la pregunta: *"¿Este artifact proviene de quien dice provenir?"*  

Sigstore opera en la capa de distribución de artifacts, no en la capa de ejecución del pipeline. No detecta ataques intermedios que modifiquen el proceso entre etapas antes de que el artifact sea firmado.  

### OWASP CI/CD Security Cheat Sheet  

La CI/CD Security Cheat Sheet provee recomendaciones y mejores prácticas para equipos de desarrollo. Responde a la pregunta: *"¿Qué debe hacer un equipo para asegurar su pipeline?"*  

La Cheat Sheet es un documento de referencia — describe qué hacer, no cómo verificar que se hizo. No provee un mecanismo formal, automatizable y verificable que valide el cumplimiento en cada transición entre etapas. No puede detectar en tiempo real que una etapa produjo un resultado diferente al declarado.  

---

## Qué hace diferente a IPVC  

IPVC introduce tres elementos que ningún marco existente provee en conjunto:  

**1. Validación formal en cada transición**  
IPVC coloca un check-point entre cada etapa del pipeline. Cada check-point compara el resultado real de la etapa contra un hash criptográfico declarado en un manifiesto de referencia antes de la ejecución. Si no coinciden, el pipeline se detiene de inmediato.  

**2. El manifiesto separado**  
El Manifiesto IPVC es un documento de referencia almacenado de forma separada del pipeline, con credenciales y permisos independientes. Esta separación significa que un atacante que comprometa el pipeline no puede simultáneamente modificar la referencia contra la cual se valida cada etapa. Esta es una propiedad de seguridad estructural que ningún marco existente implementa a nivel de pipeline.  

**3. Registro Centralizado de Escáneres**  
Uno de los problemas más fragmentados en los pipelines actuales es saber qué escáner de seguridad aplicar automáticamente para un tipo determinado de aplicación. IPVC propone un registro centralizado y clasificado de herramientas de escaneo que permite al pipeline seleccionar automáticamente el escáner adecuado, sin configuración manual por proyecto.  

---

## Resumen comparativo  

| | SLSA | Sigstore | CI/CD Cheat Sheet | OWASP IPVC |
|---|---|---|---|---|
| Protege | Artifact final | Firma del artifact | Prácticas del equipo | Flujo completo del pipeline |
| Aplica en | Etapa de build | Distribución del artifact | Documento de referencia | Cada transición entre etapas |
| Detecta ataques intermedios | Parcialmente | No | No | Sí |
| Formalmente verificable | Sí | Sí | No | Sí |
| Automatizable | Parcialmente | Sí | No | Sí |
| Referencia de confianza separada | No | No | No | Sí (Manifiesto IPVC) |

---

## Por qué debe ser un proyecto independiente  

La CI/CD Security Cheat Sheet no puede incorporar a IPVC porque:  

1. La Cheat Sheet es un **documento de referencia**, no una especificación. Agregar un protocolo formal con validación criptográfica, esquema del manifiesto, definiciones de check-points y una herramienta CLI cambiaría fundamentalmente la naturaleza del documento.

2. IPVC requiere su **propia gobernanza**: una especificación versionada, un JSON Schema para el manifiesto, una herramienta CLI (`ipvc-cli`), guías de implementación para plataformas específicas y un registro de escáneres — ninguno de estos elementos encaja dentro del formato de una cheat sheet.

3. La **audiencia es diferente**: la Cheat Sheet está dirigida a equipos de seguridad que buscan orientación. IPVC está dirigido a ingenieros DevSecOps y arquitectos de pipelines que necesitan una especificación formal e implementable.

SLSA y Sigstore no pueden incorporar a IPVC porque operan en una capa diferente del ecosistema. IPVC no es un reemplazo de ninguno de los dos — es la capa entre ellos y la ejecución del pipeline que ninguno cubre actualmente.  

---

## Materiales de referencia existentes  

Esta propuesta llega con materiales de referencia ya publicados, demostrando que el proyecto tiene una base técnica real antes de solicitar recursos de OWASP:  

- **Publicación 1** — *IPVC: Integrity Pipeline Validation and Control* — propuesta conceptual y definición del problema (Medium / GitHub)
- **Publicación 2** — *IPVC: Método de Control, Arquitectura y Especificación Técnica* — definiciones formales, pseudocódigo, propiedades criptográficas y ejemplos de implementación (Medium / GitHub)
- **Repositorio GitHub personal** — estructurado con especificación, definición formal, changelog y licencia dual (Apache 2.0 + CC BY 4.0)

---

## Conclusión  

IPVC aborda una brecha específica, formalmente definida y actualmente no cubierta en la seguridad de pipelines: la validación de integridad en cada transición entre etapas, utilizando un manifiesto separado como autoridad de referencia.  

No duplica ningún recurso OWASP existente. No compite con SLSA ni con Sigstore. Ocupa una capa del problema de seguridad de la cadena de suministro que ningún marco actual cubre.  

> **SLSA y Sigstore protegen el *qué*. La Cheat Sheet guía el *cómo*. IPVC garantiza que el *cómo* y el *cuándo* se ejecutaron exactamente como fueron declarados**.  

---

## 📜 Licencia

- *Licencia de código: [Apache 2.0](../../LICENSE.md)*
- *Licencia de documentación: [Creative Commons BY 4.0](../../LICENSE_CC.md)*

> © 2026 Fernando Flores Alvarado — IPVC (Integrity Pipeline Validation and Control)  
> *“Compartir con responsabilidad es inspirar para construir el futuro.”*  
