> ℹ️ **Nota:** Este documento es la versión en español de `owasp-proposal-IPVC.md`.
> La versión en inglés se mantiene como documento oficial para el envío a OWASP.  

# IPVC — Integrity Pipeline Validation and Control  

## Propuesta — Nuevo Proyecto OWASP  
### Etapa: Incubator  

**Autor y Project Leader propuesto:** Fernando Flores Alvarado  
**Correo OWASP:** fernando.alvarado@owasp.org  
**Fecha:** Junio 2026  
**Tipo de proyecto:** Documentación + Herramienta  
**Licencia:** Apache 2.0 (código) + CC BY 4.0 (documentación)  
**Proyecto OWASP relacionado:** [RHC — Randomized Header Channel for CSRF Protection](https://github.com/OWASP/www-project-randomized-header-channel-for-csrf-protection)  
**Estado:** Pendiente de envío formal  
---

## 1. Nombre del proyecto  

**OWASP IPVC — Integrity Pipeline Validation and Control**  

---

## 2. Descripción del proyecto  

OWASP IPVC es un método de arquitectura de seguridad que define puntos de control (*check-points*) entre cada etapa de un pipeline CI/CD, validando que el proceso, el resultado, el flujo y la integridad de cada etapa sean consistentes con lo declarado en un manifiesto de referencia definido antes de la ejecución.  

El proyecto propone:  

- Una arquitectura formal de check-points para pipelines CI/CD
- La especificación del Manifiesto IPVC como componente de referencia separado del proceso
- Un mecanismo de validación basado en hashes criptográficos por etapa
- Un registro centralizado y clasificado de herramientas de escaneo de seguridad
- Guías de implementación para los principales sistemas CI/CD (GitHub Actions, GitLab CI, Jenkins)

---

## 3. Problema que resuelve  

### Contexto  

En mayo de 2026, Andrew van der Stock, Director Ejecutivo de OWASP, alertó a la comunidad de líderes sobre un incremento significativo de ataques a la cadena de suministro de software mediante Pull Requests maliciosos, bots de IA y código ofuscado en proyectos OWASP.  

Los pipelines CI/CD actuales son flujos automatizados que no tienen mecanismos formales de validación entre etapas. Un atacante que introduce código malicioso en cualquier punto del flujo puede hacer que ese código viaje por todo el pipeline sin ser detectado, hasta llegar a producción.  

### La brecha específica  

Las preguntas que ningún mecanismo actual responde formalmente en cada transición del pipeline:  

- ¿El proceso que se ejecutó fue exactamente el esperado?
- ¿El resultado producido coincide con lo declarado previamente?
- ¿El flujo no fue alterado entre etapas?
- ¿La integridad del artifact se mantuvo desde la etapa anterior?

### Diferencia con marcos y guías existentes  

| | SLSA | Sigstore | OWASP CI/CD Cheat Sheet | OWASP IPVC |
|---|---|---|---|---|
| ¿Qué protege? | El artifact final | La firma del artifact | No protege — Guías y recomendaciones | El flujo completo del pipeline |
| ¿Dónde aplica? | Etapa de build | Distribución del artifact | Referencia para el equipo | Cada transición entre etapas |
| ¿Detecta ataques intermedios? | Parcialmente | No | No | Sí |
| ¿Es implementable automáticamente? | Parcialmente | Sí | No | Sí |

IPVC no reemplaza ni compite con los marcos y guías existentes. Los complementa.  

> **SLSA y Sigstore protegen el *qué*. La Cheat Sheet guía el *cómo*.**
> **IPVC garantiza que el *cómo* y el *cuándo* se ejecutaron exactamente como fueron declarados.**  

Los cuatro pueden coexistir y reforzarse mutuamente dentro de una arquitectura integral de seguridad para la cadena de suministro de software.  

---

## 4. Objetivos del proyecto  

### Objetivo principal  

Definir y documentar una arquitectura formal de validación por etapas para pipelines CI/CD, que sea adoptable dentro de los estándares existentes y compatible con las herramientas ya establecidas en el ecosistema.  

### Objetivos específicos  

1. Especificar formalmente el Manifiesto IPVC y su esquema de datos (JSON Schema)
2. Definir el protocolo de los check-points y su mecanismo de validación criptográfica
3. Documentar guías de implementación para GitHub Actions, GitLab CI y Jenkins
4. Especificar el Registro Centralizado de Escáneres y su protocolo de clasificación
5. Producir documentación accesible en tres niveles: básico, intermedio y avanzado
6. Desarrollar `ipvc-cli`, la herramienta de línea de comandos de referencia

---

## 5. Propuesta de valor única (USP)  

IPVC es el único marco que propone validación formal de integridad en cada transición entre etapas de un pipeline CI/CD, utilizando un manifiesto de referencia separado del proceso con permisos independientes.  

Ningún proyecto OWASP existente aborda esta capa específica del problema de la cadena de suministro. La justificación técnica detallada de por qué IPVC debe existir como proyecto independiente se presenta en el documento adjunto: **"Technical Justification for OWASP — IPVC"**.  

La especificación formal del Manifiesto IPVC ya incorpora, como parte de su diseño arquitectónico, el anclaje criptográfico al commit de origen del código fuente (control de versiones). Esto significa que la primera etapa del pipeline queda vinculada verificablemente al commit que originó la ejecución, estableciendo una cadena de custodia completa desde el código fuente hasta el artifact final desplegado — sin puntos ciegos entre el control de versiones y el proceso de validación. Esta decisión arquitectónica ya está documentada en la especificación técnica del método (ver Publicación 2 en la sección de Materiales de referencia).

---

## 6. Audiencia objetivo  

- Equipos de desarrollo que utilizan pipelines CI/CD
- Mantenedores de proyectos de código abierto (incluyendo proyectos OWASP)
- Equipos de seguridad DevSecOps
- Organizaciones que consumen recursos de la cadena de suministro de software
- Contribuidores a proyectos OWASP que gestionan pipelines automatizados

---

## 7. Tipo de proyecto  

Este proyecto es de tipo mixto:  

- **Documentación:** especificación formal del método, arquitectura, manifiesto y guías de implementación
- **Herramienta:** `ipvc-cli`, herramienta de línea de comandos para generación de huellas y validación de check-points

---

## 8. Entregables iniciales (etapa Incubator)  

Para la etapa Incubator, los entregables propuestos son:  

- [ ] Página oficial del proyecto en owasp.org
- [ ] Repositorio GitHub bajo la organización OWASP
- [ ] Especificación del Manifiesto IPVC (JSON Schema)
- [ ] Documentación del método en tres niveles (básico, intermedio, avanzado)
- [ ] Guía de implementación para GitHub Actions
- [ ] Prototipo inicial de `ipvc-cli`
- [x] Publicaciones de referencia — ya publicadas en Medium y disponibles en repositorio GitHub personal del autor

---

## 9. Relación con proyectos OWASP existentes  

### RHC — Randomized Header Channel for CSRF Protection  

El autor de IPVC es también Project Leader de RHC (etapa Incubator, ticket NFRSD-6904). La relación entre ambos proyectos es complementaria y acotada:  

En la etapa de escaneo de seguridad del pipeline, algunos escáneres realizan consultas a bases de datos de vulnerabilidades en línea (como OSV). La comunicación entre el escáner y esa fuente externa representa un límite de confianza donde RHC puede aplicarse como capa de protección. Esta es la única intersección técnica entre ambos proyectos.  

IPVC es un proyecto independiente con identidad técnica propia.  

### OWASP CI/CD Security Cheat Sheet  

La CI/CD Security Cheat Sheet de OWASP provee recomendaciones y guías para equipos de desarrollo. IPVC no duplica ese contenido — lo implementa. Mientras la Cheat Sheet describe *qué* se debe hacer, IPVC define *cómo* hacerlo de forma verificable y automatizable, con un mecanismo formal de validación entre etapas.  

La justificación extendida de esta diferenciación se desarrolla en el documento adjunto de Justificación Técnica.  

---

## 10. Liderazgo del proyecto  

De acuerdo con la política de proyectos OWASP, un nuevo proyecto requiere mínimo 2 líderes.  

| Rol | Nombre | Estado |
|---|---|---|
| Project Leader | Fernando Flores Alvarado | Confirmado — OWASP Member, Project Leader activo (RHC) |
| Co-Leader | Se iniciará una búsqueda dentro de la comunidad OWASP | Contactos a identificar en el área de DevSecOps y supply chain security |

### Enfoque de colaboración técnica  

IPVC surge desde una línea de investigación relacionada con arquitectura de software, seguridad de aplicaciones e integridad de comunicaciones.  

Si bien la propuesta aborda un problema dentro del ecosistema CI/CD y la seguridad de la cadena de suministro de software, el objetivo del proyecto no es depender exclusivamente de la experiencia de un único líder, sino construirse como un esfuerzo colaborativo dentro de la comunidad OWASP.  

Por esta razón, una de las prioridades iniciales del proyecto será incorporar colaboradores y co-líderes con experiencia práctica en áreas como DevSecOps, seguridad de pipelines, supply chain security y plataformas CI/CD modernas (GitHub Actions, GitLab CI, Jenkins, entre otras).  

Esta colaboración permitirá fortalecer la validación técnica de la propuesta, facilitar su adopción dentro de entornos reales y enriquecer la evolución del proyecto mediante la experiencia de especialistas provenientes de diferentes áreas del ecosistema de seguridad.  

El autor continuará aportando la dirección conceptual, arquitectónica y metodológica del proyecto, mientras que los co-líderes y colaboradores contribuirán con experiencia práctica, validación técnica y conocimiento especializado en plataformas y procesos CI/CD modernos.  

> **Nota:** El autor es OWASP Member activo con correo institucional confirmado (`fernando.alvarado@owasp.org`) y experiencia como Project Leader en etapa Incubator. La búsqueda de co-líder se iniciará mediante un anuncio formal en los canales de líderes y proyectos de la comunidad OWASP, con el objetivo de identificar candidatos con experiencia en DevSecOps o seguridad de cadena de suministro. De acuerdo con la política de proyectos, se completará el liderazgo mínimo dentro del período establecido tras la aprobación.  

---

## 11. Retos identificados  

La propuesta reconoce los siguientes retos que el proyecto deberá abordar:  

1. **Separación del manifiesto:** garantizar que el manifiesto no pueda ser comprometido por el mismo vector que afecta el pipeline
2. **Selección automática de escáneres:** definir el protocolo para que el pipeline identifique automáticamente el escáner correcto según el tipo de aplicación
3. **Velocidad y eficiencia:** los check-points deben tener impacto mínimo en el tiempo de ejecución del pipeline
4. **Adopción por mantenedores:** convencer a mantenedores de herramientas de escaneo existentes de registrar sus proyectos en el Registro Centralizado
5. **Compatibilidad con estándares:** asegurar que IPVC sea adoptable dentro de los marcos CI/CD ya establecidos sin requerir cambios estructurales mayores

---

## 12. Materiales de referencia disponibles  

El proyecto cuenta con materiales de referencia ya producidos antes del envío formal de esta propuesta:  

| Material | Descripción | Estado |
|---|---|---|
| Publicación 1 — Medium | Introducción al problema y propuesta conceptual de IPVC | Publicada |
| Publicación 2 — Medium | Arquitectura formal, pseudocódigo y especificación técnica | Publicada |
| Repositorio GitHub personal | Repositorio estructurado con documentación, especificación y licencias | Disponible |
| Justificación Técnica | Documento independiente de diferenciación con marcos existentes | Adjunto a esta propuesta |

---

## 13. Próximos pasos para envío formal  

- [ ] Confirmar co-líder del proyecto
- [ ] Completar el formulario oficial: [New Project Request — OWASP Contact Us](https://contact.owasp.org/)
- [ ] Preparar la página inicial del proyecto para owasp.org (requerida en los primeros 30 días tras aprobación)

---

## 14. Referencias  

- [OWASP Project Policy](https://owasp.org/www-policy/operational/projects)
- [OWASP Project Committee](https://owasp.org/www-committee-project/)
- [OWASP CI/CD Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html)
- [SLSA Framework — OpenSSF](https://slsa.dev)
- [Sigstore](https://sigstore.dev)
- [OWASP RHC — Randomized Header Channel](https://github.com/OWASP/www-project-randomized-header-channel-for-csrf-protection)
- IPVC - Una propuesta de arquitectura de seguridad para pipelines CI/CD
  - [Repositorio GitHub personal del autor](https://github.com/Fer0306/IPVC-ipvc-integrity-pipeline-validation-and-control/blob/main/docs/architecture-proposal.md)
  - [Publicación — Medium](https://medium.com/@fernandofa0306/ipvc-integrity-pipeline-validation-and-control-a88003d0c2fb)
- IPVC - Método de Control: Arquitectura y Especificación Técnica
  - [Repositorio GitHub personal del autor](https://github.com/Fer0306/IPVC-ipvc-integrity-pipeline-validation-and-control/blob/main/docs/control-method-specification.md)
  - [Publicación — Medium](https://medium.com/@fernandofa0306/ipvc-control-method-architecture-and-technical-specification-99350e44ab6d)
- Mensaje de Andrew van der Stock — Canal OWASP Leaders, 24 de mayo de 2026
  - [Enlace permanente al mensaje original en OWASP Slack (acceso restringido a miembros con autorización en el espacio de trabajo de OWASP Slack)](https://owasp.slack.com/archives/C66R5JF6V/p1779670724458619)
  - [Captura 1 — Repositorio GitHub personal del autor](https://github.com/Fer0306/IPVC-ipvc-integrity-pipeline-validation-and-control/blob/main/assets/images/project-origin/Andrew_Message_01.png)
  - [Captura 2 — Repositorio GitHub personal del autor](https://github.com/Fer0306/IPVC-ipvc-integrity-pipeline-validation-and-control/blob/main/assets/images/project-origin/Andrew_Message_02.png)

> Nota: El enlace permanente de Slack se proporciona como referencia a la fuente original. Se incluyen copias archivadas de acceso público para garantizar la accesibilidad a largo plazo y la verificación independiente de la comunicación referenciada.  

---

## 15. Sobre el autor  

Fernando Flores Alvarado es investigador independiente en ciberseguridad y OWASP Project Leader del proyecto Randomized Header Channel for CSRF Protection (RHC), actualmente en etapa Incubator (ticket NFRSD-6904, aprobado septiembre 2025).  

Su investigación se enfoca en la integridad de comunicaciones, arquitecturas distribuidas y seguridad aplicada a entornos con inteligencia artificial. Ha publicado investigación formal sobre el modelo de amenazas FCHA (Fragmented Channel Header Attack) en SSRN (Abstract ID 6808838) y tiene manuscrito enviado a la revista *Cyber Security and Applications* (Elsevier, Manuscript No. CSA-D-26-00410).  

IPVC surge como extensión natural de esa línea de investigación, aplicando los principios de integridad de canal al problema de la cadena de suministro de software y los pipelines CI/CD.  

Es miembro activo de la comunidad OWASP y participa en el canal de líderes de la fundación. El mensaje de Andrew van der Stock sobre ataques a la cadena de suministro, publicado en ese canal en mayo de 2026, fue el detonador directo de la investigación que originó IPVC.  

📫 fernando.alvarado@owasp.org  
🔗 [LinkedIn](https://www.linkedin.com/in/fernando-flores-alvarado-2786b21b8/)  
🐙 [GitHub](https://github.com/Fer0306)  
📝 [Medium](https://medium.com/@fernandofa0306/)  

---

## 📜 Licencia

- *Licencia de código: [Apache 2.0](../../LICENSE.md)*
- *Licencia de documentación: [Creative Commons BY 4.0](../../LICENSE_CC.md)*

> © 2026 Fernando Flores Alvarado — IPVC (Integrity Pipeline Validation and Control)  
> *“Compartir con responsabilidad es inspirar para construir el futuro.”*  
