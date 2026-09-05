# Portfolio de Ciberseguridad y Seguridad Ofensiva

Repositorio personal enfocado en el documentación de metodologías, laboratorios, laboratorios de certificación y apuntes técnicos en seguridad, pentesting y auditorías de infraestructura.

---

## 🛠️ Stack Técnico y Metodologías

### 1. Information Gathering & Reconnaissance (INE / eJPT)
* **Reconocimiento Pasivo:** Enumeración de DNS, footprinting de sitios web (`WhatWeb`, `Wafw00f`), harvesting de emails (`theHarvester`), Google Dorking avanzado y análisis de ficheros expuestos (`robots.txt`, `sitemap.html`).
* **Reconocimiento Activo:** Descubrimiento de hosts (`Nmap` ping sweeps, ARP, paquetes ICMP/TCP), transferencia de zonas DNS y escaneo exhaustivo de puertos/servicios (`-sV`, `-sC`, `-O`).

### 2. Web Application Pentesting & PortSwigger Labs
* **Enumeración y Fuzzing:** Descubrimiento de directorios y paneles ocultos (`Gobuster`, `Dirb`).
* **Vulnerabilidades y Explotación:** 
  * Análisis de CMS (ej. despliegue y auditoría en WordPress con `WPScan`).
  * Análisis avanzado de cabeceras, manejo de cookies y vulnerabilidades de autenticación.
  * Técnicas de *Request Smuggling* (CL.TE / TE.CL) y ataques a Web Sockets.
  * Análisis de *Web Cache Deception*.

### 3. Explotación, Post-Explotación y Active Directory (eCCPT / eJPT)
* **Servicios de Red:** Explotación de servicios comunes (FTP, SMB, SSH, HTTP, MySQL, RDP). Interacción con recursos compartidos (`smbclient`, `enum4linux`).
* **Active Directory & Windows Server:**
  * Configuración y administración de entornos Windows Server y roles de directorio.
  * Vectores de ataque orientados a credenciales (ej. *AS-REP Roasting*).
  * Movimiento lateral, técnicas de *Pivoting* y uso de herramientas como `Chisel` y `NetExec` (`nxc`).
* **Buffer Overflow (BOF):** Análisis preliminar y desarrollo de exploits (en constante expansión).

---

## 🚀 Proyectos y Prácticas Activas
* **Laboratorios eJPT:** Documentación de resolución de máquinas virtuales, análisis de credenciales, volcado de información (`/etc/passwd`, `/etc/shadow`) y esquemas de red internos.
* **PortSwigger Labs:** Prácticas activas orientadas a la vulnerabilidad web crítica.

> *Nota: Este repositorio se actualiza de manera continua conforme avanzo en mis certificaciones y laboratorios prácticos.*
