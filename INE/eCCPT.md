# eCCPT: Seguridad Ofensiva, Active Directory y Aplicaciones Web

Este documento recopila apuntes técnicos, metodologías de explotación de infraestructura, administración de entornos Windows Server[cite: 2], Active Directory[cite: 2], análisis de mecanismos de autenticación y vulnerabilidades web avanzadas (como SSRF[cite: 2] y XXE[cite: 2]) correspondientes al nivel **eCCPT**.

---

## 🏗️ 1. Windows Server & Active Directory

* **Instalación y Configuración Base:**
  * Despliegue de roles orientados a infraestructura de red[cite: 2].
  * Configuración mediante **SConfig**[cite: 2] y **Server Manager**[cite: 2].
  * Configuración de parámetros de red y asignación de **IP Fija**[cite: 2].
  * Definición y cambio de **Nombre del Equipo** mediante propiedades de sistema o Panel de Control[cite: 2].

---

## 🔐 2. Análisis de Mecanismos de Autenticación y Enumeración

* **Análisis de Tiempos de Respuesta (Timing Attacks):**
  * Identificación de usuarios válidos frente a inválidos midiendo los tiempos de procesamiento en sistemas de autenticación[cite: 2].
  * Validación rápida en respuestas por nombres de usuario no existentes frente a tiempos mayores en la validación de contraseñas de usuarios existentes[cite: 2].
* **Fuerza Bruta y Gestión de Sesiones:**
  * Ejecución de ataques de fuerza bruta sobre credenciales[cite: 2].
  * Limpieza y borrado de cookies desde el editor del navegador para restablecer bloqueos de sesión o contadores de intentos fallidos[cite: 2].

---

## 🌐 3. Vulnerabilidades Web Avanzadas

### 3.1. XML External Entity (XXE)
* Análisis de estructuras **XML**[cite: 2] y procesamiento inseguro de entidades externas para la lectura de ficheros locales del servidor o ejecución de ataques del tipo SSRF interno.

### 3.2. Server-Side Request Forgery (SSRF)
* Pruebas de concepto y vectorización de peticiones en laboratorios orientados a la elusión de restricciones de red del lado del servidor (Labs I al V)[cite: 2].

---
*Apuntes avanzados en constante actualización para reflejar escenarios complejos de Red Teaming, auditorías de Directorio Activo y seguridad en aplicaciones web.*
