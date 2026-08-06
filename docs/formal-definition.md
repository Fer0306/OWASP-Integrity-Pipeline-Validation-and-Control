> ℹ️ **Note:** This document is written in Spanish. You can use your browser to translate it into English.
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.

# 📗 Definición Formal — IPVC (Integrity Pipeline Validation and Control)  

**Autor:** Fernando Flores Alvarado  
**Proyecto:** IPVC — Integrity Pipeline Validation and Control  
**Licencia:** CC BY 4.0 (documentación)  
Información detallada sobre versiones, fechas, estado y metadatos completos, consulta [`VERSION.md`](../VERSION.md).  

---

## Sigla  

**IPVC** — Integrity Pipeline Validation and Control  

---

## Definición  

IPVC es un método de arquitectura de seguridad que introduce puntos de control (check-points) entre cada etapa de un pipeline CI/CD, validando que el proceso, el resultado, el flujo y la integridad de cada etapa sean consistentes con lo declarado en un manifiesto de referencia definido al inicio de la ejecución.  

---

## Problema que resuelve  

Los pipelines CI/CD automatizados no tienen puntos de validación entre etapas. Un atacante puede inyectar código malicioso en una etapa y este viaja sin ser detectado hasta producción. IPVC define una arquitectura que interrumpe ese flujo cuando se detecta una anomalía.  

Existen guías y recomendaciones para asegurar pipelines CI/CD — entre ellas la OWASP CI/CD Security Cheat Sheet — que describen *qué* se debe hacer. IPVC va un paso más allá: define *cómo* implementar esas recomendaciones de forma verificable, automatizable y criptográficamente comprobable en cada transición del pipeline.  

---

## Los cuatro pilares de control  

| Pilar | Pregunta que responde |
|---|---|
| **Proceso** | Lo que se ejecuta en cada etapa es exactamente lo esperado |
| **Resultado** | Lo que produce cada etapa coincide con el manifiesto |
| **Flujo** | El orden y la secuencia del pipeline no fueron alterados |
| **Integridad** | Ningún artifact fue modificado entre etapas |

---

## Los cuatro componentes del método  

| Componente | Responsabilidad | Ubicación |
|---|---|---|
| **Manifiesto IPVC** | Declarar el estado esperado de cada etapa | Sistema separado, credenciales independientes |
| **Generador de Huellas** | Calcular el hash criptográfico del resultado de cada etapa | Dentro del pipeline |
| **Check-Point** | Comparar huella real vs. esperada y evaluar reglas de validación | Entre cada etapa del pipeline |
| **Registro de Escáneres** | Centralizar y clasificar herramientas de escaneo de seguridad | Repositorio externo referenciado por el manifiesto |

---

## Componente central  

El **Manifiesto IPVC** — un documento de referencia separado del pipeline, con permisos y credenciales independientes, que declara el estado esperado de cada etapa antes de la ejecución. Los check-points comparan cada resultado real contra este manifiesto usando hashes criptográficos.  

---

## Posición dentro del ecosistema de seguridad CI/CD  

IPVC no reemplaza ni compite con marcos o guías como SLSA, Sigstore o la OWASP CI/CD Security Cheat Sheet. Es complementario a todos ellos:

- **SLSA** garantiza la trazabilidad del proceso de build.
- **Sigstore** garantiza la autenticidad del artifact mediante firma criptográfica.
- **OWASP CI/CD Cheat Sheet** provee guías y recomendaciones de buenas prácticas.
- **IPVC** implementa validación formal y automatizable en cada transición entre etapas, verificando que lo ejecutado coincide con lo declarado.

---

## Estado  

**Propuesta inicial — autoría**: Fernando Flores Alvarado, junio 2026  

---
**© 2026 Fernando Flores Alvarado — IPVC (Integrity Pipeline Validation and Control)**  
Publicado bajo [Creative Commons BY 4.0](../LICENSE_CC.md).  

> *“Compartir con responsabilidad es inspirar para construir el futuro.”*  
