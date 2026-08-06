> ℹ️ **Note:** This document is written in Spanish. You can use your browser to translate it into English.
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.

# 🌱 Origen del proyecto — IPVC (Integrity Pipeline Validation and Control)  

**Autor:** Fernando Flores Alvarado  
**Proyecto:** IPVC — Integrity Pipeline Validation and Control  
**Licencia:** CC BY 4.0 (documentación)  
Información detallada sobre versiones, fechas, estado y metadatos completos, consulta [`VERSION.md`](../VERSION.md).  

---

## El detonador  

Todo comenzó con una frase que escuché en la comunidad de seguridad: *"ataque a la cadena de suministros"*.  

Poco después, Andrew van der Stock, Director Ejecutivo de OWASP, publicó un mensaje en el canal de líderes advirtiendo sobre un incremento significativo de Pull Requests maliciosos en proyectos OWASP — generados incluso por bots de IA — con el objetivo de comprometer la cadena de suministro de software.  

Esa combinación encendió una pregunta en mi cabeza:  

> **¿Qué es esto exactamente, y cómo funciona?**  

---

## La investigación  

Me puse a investigar. No tenía experiencia previa con pipelines de CI/CD ni con la cadena de suministro de software, así que empecé desde cero.  

Lo que encontré fue esto: un pipeline CI/CD es esencialmente un flujo lineal de procesos automatizados. Cada vez que un desarrollador sube un cambio, ese flujo se activa — revisa el código, construye la aplicación, la prueba, la escanea y la despliega — sin intervención humana en cada paso.  

Al observar esa arquitectura — procesos lineales, automatizados y secuenciales — algo me llamó la atención de inmediato: **No existe ningún punto entre etapas que valide que lo que acaba de ocurrir es exactamente lo que se esperaba**. El flujo simplemente continúa.  

Ahí estaba el problema.  

---

## El momento de la decisión  

Al verlo, me dije: *"Esto es importante, hay que ayudar."*  

Y honrando el principio que guía todo mi trabajo:  

> *“Compartir con responsabilidad es inspirar para construir el futuro.”*
> — Fernando Flores Alvarado  

Me decidí a crear una solución.  

Así nació IPVC.  

### Pero tomar la decisión fue solo el principio  

De inmediato surgieron las preguntas inevitables:  

> **¿Cómo se podría validar un punto entre etapas?**

> **¿Existe alguna forma de comprobar que algo es exactamente lo que se esperaba que fuera?**

---

## El origen de la idea — BotellaControl  

IPVC no nació de la nada. Nació de la combinación natural de conceptos provenientes de proyectos que ya existían dentro de un ecosistema que llamo **BotellaControl**.  

### RHC — Randomized Header Channel  
**Proyecto OWASP:** [RHC for CSRF Protection](https://github.com/OWASP/www-project-randomized-header-channel-for-csrf-protection)  

RHC protege la integridad de los flujos de comunicación HTTP. Su principio central es simple: lo que se envía por un canal debe poder verificarse en el otro extremo. Si algo fue alterado durante el trayecto, el receptor debe poder detectarlo.  

→ **RHC aportó a IPVC: la idea de validar la integridad de lo que viaja entre dos puntos de un flujo.**  

---

### WebAI Inference Throttling  
**Repositorio:** [webai-inference-throttling](https://github.com/Fer0306/webai-inference-throttling)  

WebAI resuelve un problema diferente: cómo ejecutar modelos de Machine Learning en tiempo real en el navegador sin sobrecargar el dispositivo. La solución fue una arquitectura compuesta por cuatro capas inteligentes independientes. Cada capa decide de forma autónoma si debe actuar o no, procesando únicamente cuando existe un cambio relevante según el rol que desempeña dentro del flujo.  

Su principio central es:  **No se trata de procesar más, sino de saber cuándo no procesar.**  

→ **WebAI aportó a IPVC: la idea de estructurar un flujo mediante capas independientes capaces de tomar decisiones de forma autónoma antes de permitir que el proceso continúe.**

---

### El recuerdo de 2001 — El concepto de validación de integridad  

Aún después de identificar el problema y visualizar una posible solución, seguía existiendo una pregunta importante:  

> **¿Cómo decide cada check-point si debe permitir continuar o detener el flujo?**  

En otras palabras, aún faltaba definir el mecanismo de validación que sustentaría cada decisión.  

Mientras buscaba la respuesta, ocurrió algo inesperado.  

En esos mismos días en los que realizaba la investigación sobre pipelines CI/CD, estaba documentando uno de los laboratorios históricos de BotellaControl. Aquello me hizo recordar mis años de universidad, alrededor de 2001, cuando descargaba distribuciones Linux utilizando una conexión telefónica de 56K.  

En aquella época una instalación podía requerir varias imágenes ISO y la descarga completa podía tardar días. Por esa razón era habitual verificar que cada archivo se hubiera descargado correctamente antes de utilizarlo.  

El procedimiento era simple.  

Una herramienta generaba una cadena de comprobación a partir del archivo descargado y esa cadena debía coincidir exactamente con la publicada por el autor. Si coincidía, existía una razonable certeza de que el archivo era íntegro. Si no coincidía, algo había fallado durante la descarga o el archivo había sido alterado.  

Al recordar ese mecanismo entendí algo importante.  

*El patrón era exactamente el mismo que necesitaba para validar cada etapa de IPVC*.

Cada etapa podía producir evidencia verificable de su ejecución y el siguiente check-point podía comparar esa evidencia contra lo que se esperaba obtener antes de permitir que el flujo continuara. La idea parecía tener sentido, pero necesitaba comprobar si era posible aplicarla dentro de una arquitectura basada en puntos de control.  

Fue entonces cuando volví a revisar WebAI Inference Throttling. Al observar nuevamente su arquitectura noté algo interesante. Lo que parecía evidente pasó a convertirse en una observación consciente:  

> **Las cuatro capas ya funcionaban como puntos de control independientes.**  

Cada una evaluaba una condición específica antes de permitir que el flujo continuara.  

En ese momento comprendí algo que no había visto antes.  

Ya había construido una arquitectura basada en puntos de control, sin haber relacionado ese enfoque con el concepto de validación de integridad que acababa de recordar.

*La estructura ya estaba ahí*.  

Lo único que faltaba era incorporar un mecanismo de validación que sustentara la decisión de cada capa.  

**Y fue entonces cuando ambas ideas encajaron**.  

- La arquitectura de WebAI proporcionaba la estructura de control.
- El recuerdo de 2001 proporcionaba el mecanismo de validación de integridad.

Juntas resolvían exactamente el problema que había identificado durante la investigación de los pipelines CI/CD.  

*IPVC ya tenía una estructura de checkpoints*.  

*Ahora también tenía una forma de validar cada uno de ellos antes de permitir que el flujo continuara*.  

→ **WebAI aportó la estructura de puntos de control.**

→ **El recuerdo de 2001 aportó el concepto de validación de integridad que permite a esos puntos de control tomar una decisión verificable.**

---

## La síntesis — IPVC  

De RHC tomé la integridad. De WebAI tomé la arquitectura basada en puntos de control independientes. Y de aquel recuerdo de 2001 tomé el concepto de validación de integridad como mecanismo de decisión.  

Al combinar los tres enfoques surgió IPVC aplicado al problema de los pipelines CI/CD.  

> **Control de un flujo de procesos mediante validación continua de integridad.**  

Cada check-point en IPVC es una capa de control que decide de forma autónoma si el flujo puede continuar, basándose en evidencia verificable producida por la etapa anterior.  

---

## El componente adicional — Registro Centralizado de Escáneres  

Durante la investigación encontré otro problema. Existe una gran cantidad de herramientas de escaneo de seguridad desarrolladas por emprendedores, investigadores y proyectos de la comunidad. Sin embargo, estas herramientas se encuentran dispersas, sin clasificación homogénea y sin un lugar centralizado donde descubrirlas fácilmente.  

Si alguien desea saber qué escáner utilizar para un tipo específico de aplicación, normalmente debe invertir tiempo buscando entre múltiples fuentes, sin garantía de encontrar la herramienta más adecuada.  

IPVC propone resolver este problema mediante un **Registro Centralizado de Escáneres**.  

Un único lugar donde las herramientas puedan clasificarse por tipo de aplicación, categoría de análisis y propósito de seguridad. Esto permitiría que un pipeline seleccionara automáticamente las herramientas apropiadas para cada escenario, reduciendo la dependencia de configuraciones manuales.  

Además, abre la posibilidad de que proyectos existentes puedan integrarse dentro de un ecosistema común en lugar de operar de forma completamente aislada.  

---

## Los tres proyectos — una visión de conjunto  

| Proyecto  | Qué protege                                | Cómo lo hace                                    |
| --------- | ------------------------------------------ | ----------------------------------------------- |
| **RHC**   | La integridad de un flujo de comunicación  | Validando lo que viaja entre cliente y servidor |
| **WebAI** | La eficiencia de un flujo de procesamiento | Controlando cuándo cada capa actúa              |
| **IPVC**  | La integridad de un flujo de procesos      | Validando cada etapa antes de continuar         |

Los tres nacen de un principio común: **Un flujo sin control es un flujo vulnerable.**  

---
**© 2026 Fernando Flores Alvarado — IPVC (Integrity Pipeline Validation and Control)**  
Publicado bajo [Creative Commons BY 4.0](../LICENSE_CC.md).  

> *“Compartir con responsabilidad es inspirar para construir el futuro.”*  
