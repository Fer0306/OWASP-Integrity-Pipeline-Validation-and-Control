> ℹ️ **Note:** This document is written in Spanish. You can use your browser to translate it into English.
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.

# 🎓 Principios Fundamentales — IPVC (Integrity Pipeline Validation and Control)  

**Autor:** Fernando Flores Alvarado  
**Proyecto:** IPVC — Integrity Pipeline Validation and Control  
**Licencia:** CC BY 4.0 (documentación)  
Información detallada sobre versiones, fechas, estado y metadatos completos, consulta [`VERSION.md`](../VERSION.md).  

---

# 1. Propósito

Este documento establece los principios fundamentales que definen el método **Integrity Pipeline Validation and Control (IPVC)**.

Estos principios constituyen la base conceptual y arquitectónica sobre la cual se desarrollan las especificaciones, implementaciones y componentes futuros del proyecto.

---

# 2. Componentes Fundamentales

IPVC se compone de cuatro elementos fundamentales:

1. Manifiesto IPVC
2. Generador de Huellas
3. Check-Points
4. Registro de Escáneres

Estos cuatro componentes trabajan conjuntamente para verificar que cada transición del pipeline mantenga el estado esperado definido previamente.

---

# 3. Manifiesto IPVC

El **Manifiesto IPVC** es un documento de referencia creado antes de la ejecución del pipeline.

Su función es definir el estado esperado del proceso y servir como base para todas las validaciones posteriores.

El Manifiesto puede incluir:

* Etapas esperadas del pipeline
* Artifacts esperados
* Reglas de validación
* Huellas criptográficas de referencia (hashes)
* Hash de origen del código fuente (commit hash de control de versiones)
* Metadatos de versión
* Información de contexto del pipeline

El Manifiesto debe almacenarse de forma independiente al entorno donde se ejecuta el pipeline.

---

# 4. Generador de Huellas

El **Generador de Huellas** es el componente responsable de calcular la huella digital (hash criptográfico) del resultado producido por cada etapa del pipeline.

Esta huella representa el estado exacto del artifact o proceso en el momento de su generación, y es el valor que el Check-Point comparará contra el Manifiesto IPVC.

El Generador de Huellas opera dentro del pipeline y produce un valor determinista a partir del contenido real de cada etapa.

---

# 5. Check-Points

Un **Check-Point** es una capa de validación insertada entre dos etapas consecutivas del pipeline.

Cada Check-Point compara el estado real del proceso contra el estado esperado definido en el Manifiesto IPVC.

Todo Check-Point debe validar los siguientes pilares:

## Proceso

¿La tarea o proceso ejecutado corresponde exactamente al esperado?

## Resultado

¿La salida generada coincide con lo declarado previamente?

## Flujo

¿La secuencia de ejecución del pipeline se mantiene sin alteraciones?

## Integridad

¿El artifact generado conserva la integridad declarada en el Manifiesto?

---

# 6. Registro de Escáneres

El **Registro de Escáneres** es un repositorio centralizado y clasificado de herramientas de escaneo de seguridad disponibles para pipelines CI/CD.

Su función es permitir que el pipeline seleccione automáticamente la herramienta de escaneo adecuada según el tipo de aplicación que se está construyendo, sin requerir configuración manual por proyecto.

El Registro de Escáneres es referenciado por el Manifiesto IPVC y se mantiene como componente externo independiente.

---

# 7. Estados de Validación

Cada Check-Point debe producir uno de los siguientes estados:

| Estado  | Descripción                            |
| ------- | -------------------------------------- |
| PASS    | La validación fue exitosa              |
| FAIL    | La validación detectó una discrepancia |
| BLOCKED | No fue posible completar la validación |
| WARNING | Se detectó una anomalía no crítica     |

---

# 8. Comportamiento del Pipeline

Si un Check-Point devuelve el estado **FAIL**, el pipeline debe:

* Detener inmediatamente su ejecución.
* Bloquear el despliegue a etapas posteriores.
* Generar un reporte de validación.
* Registrar el evento para análisis posterior.

La continuación del proceso solo debe ocurrir después de que la anomalía haya sido revisada y resuelta.

---

# 9. Validación Criptográfica

La integridad de los artifacts debe verificarse mediante funciones hash criptográficas.

Ejemplos:

* SHA-256
* SHA-384
* SHA-512

El algoritmo utilizado debe estar declarado explícitamente dentro del Manifiesto IPVC.

Las comparaciones realizadas por los Check-Points deben utilizar los valores de referencia definidos previamente en dicho Manifiesto.

---

# 10. Separación de Confianza

El repositorio, almacenamiento o ubicación del Manifiesto IPVC debe mantenerse separado del entorno de ejecución del pipeline.

El compromiso de un componente del pipeline no debe implicar automáticamente el compromiso del Manifiesto de referencia.

La separación de confianza constituye uno de los principios fundamentales de IPVC.

---

# 11. Principios Fundamentales

Toda implementación de IPVC debe preservar los siguientes principios:

1. **Validación antes de continuidad**
   Ninguna etapa debe continuar sin haber sido validada previamente.

2. **Referencia externa de confianza**
   La validación debe realizarse contra un estado esperado definido antes de la ejecución.

3. **Integridad verificable**
   Los artifacts deben poder verificarse mediante mecanismos criptográficos.

4. **Separación de responsabilidades**
   El proceso validado y el mecanismo de validación no deben depender del mismo límite de confianza.

5. **Detención ante discrepancias**
   Una discrepancia crítica debe impedir la continuación automática del pipeline.

6. **Anclaje de origen**
   La primera etapa del pipeline debe estar criptográficamente vinculada al hash de origen del código fuente (commit hash), estableciendo el punto de partida verificable de toda la cadena de custodia.

---
**© 2026 Fernando Flores Alvarado — IPVC (Integrity Pipeline Validation and Control)**  
Publicado bajo [Creative Commons BY 4.0](../LICENSE_CC.md).  

> *“Compartir con responsabilidad es inspirar para construir el futuro.”*  
