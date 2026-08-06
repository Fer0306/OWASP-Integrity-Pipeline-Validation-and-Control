> ℹ️ **Note:** This document is written in Spanish. You can use your browser to translate it into English.
> The Spanish version is preserved intentionally as part of the project's authorship and intellectual identity.

# 🧭 Roadmap 2026 — IPVC (Integrity Pipeline Validation and Control)  

**Autor:** Fernando Flores Alvarado  
**Proyecto:** IPVC — Integrity Pipeline Validation and Control  
**Licencia:** CC BY 4.0 (documentación)  
Información detallada sobre versiones, fechas, estado y metadatos completos, consulta [`VERSION.md`](../VERSION.md).  

---

## 🎯 Propósito  

El presente roadmap define la hoja de ruta del **IPVC — Integrity Pipeline Validation and Control**, un proyecto de investigación aplicada a los pipelines CI/CD, integrando un método de arquitectura de seguridad que introduce puntos de control (*check-points*) entre cada etapa de un pipeline CI/CD, validando que el proceso, el resultado, el flujo y la integridad de cada etapa sean consistentes con lo declarado en un **Manifiesto de referencia** definido antes de la ejecución.  

Busca su integración con OWASP y su posterior difusión.  

El historial completo de cambios por archivo está disponible en el historial de commits de Git.  

---

## 🧩 Fase 1 — Estructura base y documentación (Q1 2026)  

**Objetivo:**  
Establecer los cimientos técnicos y documentales del proyecto, alineados con estándares OWASP.  

**Tareas clave:**  
- ✅ Definición de estructura del repositorio (assets, docs, roadmap, publications)
- ✅ Redacción de documentación base (`README.md`, `architecture-proposal.md`, `control-method-specification.md`, `formal-definition.md`, `project-origin.md`, `repository-structure.md`, `fundamental-principles.md`, `article_links.md`)
- ✅ Creación de esquema dual de licencias (Apache 2.0 + CC BY 4.0)
- ✅ Publicación inicial en GitHub y validación estructural
- ✅ Creación de la publicación 1 — Publicada durante la fase inicial del proyecto.
- ✅ Creación de la publicación 2 — Publicada posteriormente como parte de las actividades de difusión y presentación de la propuesta.
- ✅ Redactar la propuesta formal de este proyecto para OWASP

**Resultado esperado:**  
Repositorio IPVC — Integrity Pipeline Validation and Control completamente documentado y funcional.  

### 🗂️ Cronología de investigación y difusión

Durante la fase de investigación y construcción inicial del repositorio IPVC, se desarrollaron de forma paralela diversos materiales técnicos y de divulgación.

#### Investigación y documentación

* Desarrollo de la propuesta formal para OWASP.
* Elaboración de la justificación técnica del proyecto.
* Redacción de la documentación fundacional del método IPVC.
* Definición del modelo de licenciamiento dual:

  * Apache License 2.0 para código y pruebas de concepto.
  * Creative Commons BY 4.0 para documentación.

#### Difusión pública

Las publicaciones relacionadas con IPVC fueron redactadas durante el proceso de investigación y construcción del repositorio, aunque su publicación pública se realizó de forma gradual conforme avanzó la madurez del proyecto.

* Publicación 1 — Publicada durante la fase inicial del proyecto.
* Publicación 2 — Publicada posteriormente como parte de las actividades de difusión y presentación de la propuesta.

Esta cronología permite diferenciar claramente entre la fecha de creación de los materiales y la fecha de su publicación pública.

---

## 🔬 Fase 2 — Preparación de propuesta para OWASP (Q2 2026)  

**Objetivo:**  
Presentar la **propuesta formal** ante el comité de proyectos OWASP, con documentación de diferenciación técnica completa.  

**Tareas clave:**  
- ✅ Redacción de propuesta formal OWASP (borrador v2.0)
- ✅ Elaboración de `docs/owasp/technical-justification.md` — diferenciación técnica respecto a SLSA, Sigstore y OWASP CI/CD Security Cheat Sheet
- ✅ Incorporación de la OWASP CI/CD Security Cheat Sheet como marco de referencia en la arquitectura de diferenciación
- 🔲 Confirmación de co-líder del proyecto (búsqueda activa en comunidad OWASP DevSecOps)
- 🔲 Envío formal mediante formulario oficial: [New Project Request — OWASP Contact Us](https://contact.owasp.org/)
- 🔲 Esperar la respuesta y validación del comité de OWASP

**Resultado esperado:**  
Validación positiva del comité de OWASP e inicio formal del proyecto en etapa Incubator.  

---

## 🔎 Fase 3 — Integración y alineación (Q3 2026)  

**Objetivo:**  
Desarrollar los componentes técnicos formales del método y establecer compatibilidad con los principales sistemas CI/CD.  

**Tareas clave:**  
- 🔲 Exploración de compatibilidad con GitHub Actions, GitLab CI y Jenkins
- 🔲 Especificación formal del esquema del Manifiesto IPVC (JSON Schema / OpenAPI)
- 🔲 Definición de la interfaz de línea de comandos `ipvc-cli`
- 🔲 Especificación del Registro de Escáneres y su protocolo de clasificación
- 🔲 Estudio de compatibilidad con SLSA attestations como fuente de hashes
- 🔲 Propuesta formal para adopción como proyecto OWASP
- 🔲 Inicio de guía de implementación para GitHub Actions (primer sistema CI/CD objetivo)

**Resultado esperado:**  
Especificaciones técnicas formales listas para implementación. Primer borrador de guía CI/CD.  

---

## ⚙️ Fase 4 — Implementación de referencia (Q4 2026)  

**Objetivo:**  
Producir los primeros entregables implementables del método IPVC.  

**Tareas clave:**  
- 🔲 Prototipo inicial de `ipvc-cli` (generación de huellas y validación de check-points)
- 🔲 Guía de implementación completa para GitHub Actions
- 🔲 Publicación de especificación JSON Schema del Manifiesto IPVC
- 🔲 Documentación de tres niveles (básico, intermedio, avanzado) para adopción externa
- 🔲 Página oficial del proyecto en owasp.org

**Resultado esperado:**  
Primera versión funcional de `ipvc-cli`. Documentación lista para adopción por equipos DevSecOps.  

---

## ✨ Cierre simbólico  

> *"Cada punto de control (check-points) es una capa de conciencia en la defensa."* 
> — Fernando Flores Alvarado  

---
**© 2026 Fernando Flores Alvarado — IPVC (Integrity Pipeline Validation and Control)**  
Publicado bajo [Creative Commons BY 4.0](../LICENSE_CC.md).  

> *“Compartir con responsabilidad es inspirar para construir el futuro.”*
