> ℹ️ **Note for English readers:**  
> This project is originally written in Spanish, as it is the author's native language and part of its conceptual origin.  
> For accessibility, you can use your browser’s built-in translation (Chrome, Edge, etc.) to read it in English.  
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.

# IPVC — Integrity Pipeline Validation and Control  

> Una propuesta de arquitectura de seguridad para pipelines CI/CD  

---

**Autor:** Fernando Flores Alvarado  
**Licencia:** Apache 2.0 (código) + CC BY 4.0 (documentación)  

Este repositorio contiene la documentación y materiales de referencia
de la propuesta IPVC (Integrity Pipeline Validation and Control),
actualmente en preparación para su presentación formal a OWASP.  

Para información detallada sobre versiones, fechas, estado del proyecto y metadatos completos, consulta el archivo [`VERSION.md`](./VERSION.md).  

---

## ¿Qué es IPVC?  

IPVC es un método de arquitectura de seguridad que introduce puntos de control (*check-points*) entre cada etapa de un pipeline CI/CD, validando que el proceso, el resultado, el flujo y la integridad de cada etapa sean consistentes con lo declarado en un **Manifiesto de referencia** definido antes de la ejecución.  

Si cualquier check-point detecta una discrepancia, el pipeline se detiene y se genera un reporte. El proceso no continúa hasta que la anomalía sea resuelta.  

---

## ¿Por qué IPVC?  

### El problema que resuelve  

Los pipelines CI/CD actuales automatizan el proceso de entrega de software a través de múltiples etapas:  

```text
Pull Request → Build → Pruebas → Escaneo de Seguridad → Deploy
```

Aunque existen controles de seguridad dentro de algunas etapas, y existen guías y recomendaciones para implementarlos, no existe un mecanismo formal que valide la integridad de la *transición entre etapas* — verificando que lo que entró a una etapa y lo que salió de ella es exactamente lo que se declaró antes de la ejecución.  

Un atacante que logra comprometer una etapa puede hacer que código o artifacts maliciosos recorran todo el pipeline sin ser detectado, hasta llegar a producción.  

IPVC busca cerrar esa brecha.  

---

## Los cuatro pilares de control  

| Pilar      | Pregunta que responde                               |
| ---------- | --------------------------------------------------- |
| **Proceso**    | ¿Lo que se ejecutó fue exactamente lo esperado?                      |
| **Resultado**  | ¿Lo que produjo la etapa coincide con lo declarado en el manifiesto? |
| **Flujo**      | ¿El orden y la secuencia del pipeline se mantuvieron intactas?       |
| **Integridad** | ¿Los artifacts permanecieron sin modificaciones?                     |

---

## 🧩 El flujo con check-points  

```text
Git (commit_hash_origen)
         │
         ▼
   Desarrollador
         │
         ▼
    Pull Request
         │
         ▼
 [CHECK-POINT] ← valida contra manifiesto (incluye commit_hash_origen)
         │
         ▼
       Build
         │
         ▼
 [CHECK-POINT] ← valida contra manifiesto
         │
         ▼
     Pruebas
         │
         ▼
 [CHECK-POINT] ← valida contra manifiesto
         │
         ▼
 Escaneo de Seguridad
         │
         ▼
 [CHECK-POINT] ← valida contra manifiesto
         │
         ▼
      Deploy
```

---

## Diferencia con marcos y guías existentes  

| | SLSA | Sigstore | OWASP CI/CD Cheat Sheet | IPVC |
|---|---|---|---|---|
| ¿Qué protege?                      | El artifact final | La firma del artifact     | No protege — Guías y recomendaciones | El flujo completo del pipeline |
| ¿Dónde aplica?                     | Etapa Build       | Distribución del artifact | Documento de referencia              | Cada transición entre etapas   |
| ¿Detecta ataques intermedios?      | Parcialmente      | No                        | No                                   | Sí                             |
| ¿Es implementable automáticamente? | Parcialmente      | Sí                        | No                                   | Sí                             |

IPVC no reemplaza ni compite con los marcos y guías existentes. Los complementa.  

> **SLSA y Sigstore protegen el *qué*. La Cheat Sheet guía el *cómo*.**
> **IPVC garantiza que el *cómo* y el *cuándo* se ejecutaron exactamente como fueron declarados.**  

Los cuatro pueden coexistir y reforzarse mutuamente dentro de una arquitectura integral de seguridad para la cadena de suministro de software.  

---

## 📘 Documentos del proyecto  

| Documento | Descripción |
|---|---|
| [docs/architecture-proposal.md](./docs/architecture-proposal.md)     | Propuesta de arquitectura de seguridad |
| [docs/control-method-specification.md](./docs/control-method-specification.md) | Definición Formal del método de Control |
| [docs/formal-definition.md](./docs/formal-definition.md)             | Definición formal del método           |
| [docs/fundamental-principles.md](./docs/fundamental-principles.md)   | Principios Fundamentales               |

---

## 🏛️ Documentación para OWASP

| Documento | Descripción |
|------------|------------|
| [docs/owasp/owasp-project-proposal.md](./docs/owasp/owasp-project-proposal.md) | Propuesta formal presentada a OWASP |
| [docs/owasp/technical-justification.md](./docs/owasp/technical-justification.md) | Justificación técnica |

---

### 🌐 Publicaciones externas  

> Medium:
> [`publications/medium/article_links.md`](./publications/medium/article_links.md)  

---

## 📊 Estado del proyecto  

| Componente                                   | Estado                 |
| -------------------------------------------- | ---------------------- |
| Definición conceptual                        | ✅ Completado          |
| Primera Publicación — propuesta              | ✅ Publicada           |
| Segunda Publicación — especificación técnica | ✅ Publicada           |
| Propuesta OWASP                              | ✅ Lista               |
| Especificación del manifiesto IPVC           | 🔲 Pendiente           |
| Herramienta `ipvc-cli`                       | 🔲 Pendiente           |
| Guías CI/CD                                  | 🔲 Pendiente           |

---

## Sobre el autor

Fernando Flores Alvarado es investigador independiente en ciberseguridad y líder del proyecto OWASP [Randomized Header Channel for CSRF Protection (RHC)](https://github.com/OWASP/www-project-randomized-header-channel-for-csrf-protection).

Su trabajo se enfoca en la integridad de comunicaciones, arquitecturas distribuidas y seguridad aplicada a entornos con inteligencia artificial.

IPVC surge como línea de investigación, enfocándose en la integridad de los procesos dentro de la cadena de suministro de software y los pipelines CI/CD.

---

## 🌱 Origen del proyecto

Este proyecto nació en español.  

Como autor, desarrollador e investigador, la concepción original y la documentación fueron pensadas y construidas en español como lenguaje base.  

Este origen no es casual. Es parte de una identidad y de un propósito.  

> Para más contexto, consulta:
> [`docs/project-origin.md`](./docs/project-origin.md)  

---

## ⚖️ Licencias  

| Componente | Licencia |
|---|---|
| Código fuente | *[Apache License 2.0](./LICENSE.md)* |
| Documentación, textos, imágenes y diagramas | *[Creative Commons BY 4.0](./LICENSE_CC.md)* |

> Este proyecto utiliza un esquema de licenciamiento dual.  

---
**© 2026 Fernando Flores Alvarado — Todos los derechos reservados bajo las licencias indicadas.**  

> *“Compartir con responsabilidad es inspirar para construir el futuro.”*
