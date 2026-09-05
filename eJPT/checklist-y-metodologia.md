# eJPT & INE: Metodología y Checklist Profesional de Pentesting

Este documento consolida los apuntes técnicos, metodologías, comandos y flujos de trabajo prácticos derivados de la certificación **eJPT (eLearnSecurity Junior Penetration Tester)** y los módulos de **INE**, estructurados de forma modular, profesional y lista para ser integrada en GitHub.

---

## 📋 1. Metodología General de Evaluación

Una auditoría de seguridad ofensiva o prueba de penetración se rige bajo un ciclo estructurado para garantizar la cobertura total de la superficie de ataque:

1. **Reconocimiento y Footprinting (Pasivo y Activo):** Identificación de la infraestructura expuesta, dominios, subdominios y servicios activos sin interactuar directamente con la lógica interna crítica de forma intrusiva al inicio.
2. **Escaneo y Enumeración:** Descubrimiento minucioso de puertos, versiones de servicios, directorios ocultos, recursos compartidos y configuraciones incorrectas.
3. **Análisis de Vulnerabilidades y Evaluación Web:** Identificación de fallas comunes (OWASP Top 10), inyecciones, problemas de autenticación o vulnerabilidades específicas de CMS (`WPScan`, etc.).
4. **Explotación:** Validación de vectores de ataque mediante la ejecución controlada de exploits (manuales, Metasploit o scripts personalizados).
5. **Post-Explotación y Escalada de Privilegios:** Extracción de credenciales, dumping de información sensible, análisis de permisos en sistemas Linux/Windows, movimiento lateral y establecimiento de persistencia.
6. **Documentación y Reporte:** Registro exhaustivo de evidencias, comandos ejecutados, flags obtenidas y recomendaciones de remediación.

---

## 🔎 2. Reconocimiento y Escaneo (Information Gathering)

### 2.1. Reconocimiento Pasivo
* **Footprinting Web y Análisis de Infraestructura:**
  * Archivos clave a revisar: `/robots.txt`, `/sitemap.html`.
  * Herramientas de huella digital: `WhatWeb`, `Wafw00f` (`wafw00f site -a` para detección de WAF).
  * Extracción de metadatos y ficheros: `HTTrack` (`httrack http://target.ine.local -O INE`).
  * Descubrimiento de subdominios: `sublist3r -d dominio.com`.
  * Recopilación de correos electrónicos: `theHarvester`.
  * **Google Dorks útiles:**
    ```text
    inurl:admin
    inurl:forum
    inurl:passwd.txt
    intitle:index of
    filetype:pdf
    site:*.gob.ar
    ```

### 2.2. Reconocimiento Activo y Escaneo de Puertos
* **Descubrimiento de Hosts:**
  * Barrido Ping / ARP / ICMP / TCP SYN (`-PS`, `-Pn`).
* **Nmap - Optimización y Fases:**
  * Escaneo rápido inicial de puertos abiertos (sin resolución de versiones pesadas para ahorrar tiempo):
    ```bash
    nmap -p- --min-rate=1000 -T4 <TARGET-IP>
    ```
  * Escaneo detallado enfocado en servicios y scripts por defecto sobre los puertos descubiertos:
    ```bash
    nmap -sS -sV -sC -O -p <PUERTOS_ABIERTOS> <TARGET-IP>
    ```
  * Opciones avanzadas de evasión y control:
    * `-sA` (ACK scan para análisis de filtrado por Firewall).
    * `-f` (Fragmentación de paquetes).
    * `-D` (Decoy / suplantación de origen para confundir sistemas IDS/Firewall).
  * **Resolución de nombres locales:** Si un servicio web responde con un error de dominio o `Site not found`, verificar configuraciones añadiendo la IP y el dominio virtual en `/etc/hosts`:
    ```bash
    sudo nano /etc/hosts
    # Añadir: <IP> <dominio_interno>
    ```

---

## 🌐 3. Enumeración de Servicios y Aplicaciones Web

### 3.1. Servicios de Red Comunes
* **FTP (Puerto 21):**
  * Verificar acceso por credenciales anónimas (`anonymous` / `anonymous`).
  * Conexión interactiva: `ftp <TARGET-IP>`
* **SMB / NetBIOS (Puertos 139 / 445):**
  * Listado de recursos compartidos: `smbclient -L //<TARGET-IP>`
  * Enumeración profunda de usuarios y recursos: `enum4linux <TARGET-IP>`
  * Ataques de relay y validación de políticas de compartición.
* **HTTP / HTTPS (Puertos 80 / 443):**
  * Fuzzing de directorios y archivos ocultos (con soporte para recursividad en rutas 403 o subdirectorios):
    ```bash
    gobuster dir -u http://<TARGET-IP> -w /path/to/wordlist.txt
    ```
  * Análisis automatizado con Nikto:
    ```bash
    nikto -h http://<TARGET-IP>
    ```

### 3.2. Auditoría de Aplicaciones Web y CMS
* **WordPress (`WPScan`):**
  * Enumeración de usuarios, plugins vulnerables y temas expuestos:
    ```bash
    wpscan --url [https://sitio.com](https://sitio.com) --api-token <API_TOKEN> --enumerate all
    ```
* **Vulnerabilidades Web Clave a Evaluar:**
  * **SQL Injection (SQLi):** Pruebas basadas en errores, booleanas y *Union-based* (`sqlmap`).
  * **Local File Inclusion (LFI):** Búsqueda de parámetros vulnerables (`page`, `file`, `include`) apuntando a ficheros del sistema como `/etc/passwd` o envenenamiento de logs (*Log Poisoning* para lograr RCE).
  * **File Upload Bypasses:**
    * Manipulación de la cabecera `Content-Type` o `Content-Disposition`.
    * Bypass de filtros por extensión mediante técnicas de archivos anulados (`.htaccess`) o inyección de código políglota en metadatos de imágenes (`exiftool`).

---

## 🎯 4. Explotación y Post-Explotación

### 4.1. Uso de Metasploit Framework (MSF)
* Inicialización de base de datos y organización por workspaces:
  ```bash
  service postgresql start
  msfconsole
  workspace -a <nombre_proyecto>
