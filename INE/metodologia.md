# INE: Ciberseguridad e Infraestructura de Red

Este documento consolida los apuntes técnicos, metodologías, fases de reconocimiento, explotación y post-explotación (tanto en entornos Linux como Windows/Active Directory) provenientes de los laboratorios y módulos de formación de **INE**.

---

## 🔎 1. Information Gathering y Metodología de Reconocimiento

### 1.1. Reconocimiento Pasivo (Footprinting)
* **Análisis Web y Tecnologías:**
  * Identificación de CMS, servidores web y tecnologías con `WhatWeb`, `BuiltWith` o herramientas de huella digital.
  * Análisis de ficheros clave expuestos: `/robots.txt`, `/sitemap.html`.
  * Clonación y espejado de sitios web para análisis offline: `HTTrack` (`httrack http://target.ine.local -O INE`).
* **Descubrimiento de Subdominios y Correos:**
  * Enumeración de subdominios: `sublist3r -d dominio.com`.
  * Recopilación de información y correos corporativos: `theHarvester`.
  * **Google Dorks avanzados:**
    ```text
    inurl:admin
    inurl:forum
    inurl:passwd.txt
    intitle:index of
    filetype:pdf
    site:*.gob.ar
    ```
* **Footprinting de Red / DNS:**
  * Consultas DNS, registros TXT/MX y transferencia de zonas.

### 1.2. Reconocimiento Activo y Escaneo Optimizado
* **Host Discovery (Nmap):**
  * Barrido de red dentro de subredes aisladas para identificar equipos activos.
* **Escaneo de Puertos Eficiente (Metodología de 2 Fases):**
  * *Fase 1 (Descubrimiento rápido sin scripts ni versiones pesadas):*
    ```bash
    nmap -p- --min-rate=1000 -T4 <TARGET-IP>
    ```
  * *Fase 2 (Escaneo profundo enfocado exclusivamente en los puertos abiertos descubiertos):*
    ```bash
    nmap -sS -sV -sC -O -p <PUERTOS_ABIERTOS> <TARGET-IP>
    ```
* **Resolución de Nombres en Entornos de Laboratorio:**
  * Mapeo manual de hosts en sistemas Linux cuando una web requiere resolución de nombres:
    ```bash
    sudo nano /etc/hosts
    # Añadir: <IP> <dominio_interno>
    ```

---

## 🌐 2. Enumeración Avanzada de Servicios y Fuzzing Web

### 2.1. Servicios de Red y Explotación Base
* **FTP & SMB:**
  * Verificación de acceso anónimo e interacción mediante `ftp`.
  * Listado de recursos compartidos con `smbclient` y enumeración con `enum4linux`.
* **Fuzzing Web Profesional (Gobuster, Dirsearch, Ffuf):**
  * Búsqueda recursiva de directorios y ocultamiento de respuestas vacías o códigos específicos:
    ```bash
    gobuster dir -u [https://www.dominio.com/](https://www.dominio.com/) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -k -b 404 -x php,html,txt --add-slash
    ```
  * Uso de `ffuf` para filtrado avanzado por código de estado HTTP (`-mc 200,301,302,403`):
    ```bash
    ffuf -u http://target/FUZZ -w wordlist.txt -mc 200
    ```

### 2.2. Vulnerabilidades Web y Explotación de Aplicaciones
* **SQL Injection (SQLi):**
  * Técnicas *In-Band* (Union-based) para extracción de esquemas de bases de datos, volcado de tablas, lectura de ficheros del sistema operativo (`/etc/passwd`, `/var/www/html/config.php`) o inyección mediante cabeceras HTTP.
  * *Error-based SQLi* mediante la manipulación condicional de consultas.
* **File Inclusion y Subida de Ficheros (LFI / RCE / Upload Bypasses):**
  * Explotación de parámetros vulnerables (`page`, `file`, `include`) orientados a lectura de ficheros críticos o *Log Poisoning* para lograr ejecución remota de comandos (RCE).
  * Bypass de restricciones por tipo MIME (`Content-Type`, `Content-Disposition`) o extensión para la subida de webshells.
  * Generación de archivos políglotas con metadatos incrustados (`exiftool`).

---

## 💻 3. Post-Explotación en Sistemas Linux y Windows

### 3.1. Enumeración en Entornos Linux (`sysinfo`, privilegios)
* Verificación de sesiones activas e interacción con intérpretes de comandos: `/bin/bash -i`.
* Auditoría de permisos de usuario, grupos, binarios SUID y tareas programadas en el sistema.

### 3.2. Enumeración y Auditoría en Windows (Local & Active Directory)
* Extracción sistemática de información del sistema, parches instalados, variables de entorno y servicios en ejecución.
* Uso de herramientas de automatización de auditoría local como **JAWS** o **winPEAS** para la identificación rápida de vectores de escalada de privilegios locales.

---

## ⚡ 4. Active Directory y Movimiento Lateral (Módulos Avanzados)

### 4.1. Automatización con NetExec / CrackMapExec (`nxc` / `cme`)
* **Validación Masiva y Análisis:**
  * Enumeración masiva de recursos compartidos, detección de firmas SMB y verificación de políticas de contraseñas (`--pass-pol`).
  * Ejecución de ataques de *Password Spraying* y volcado de credenciales en dominios corporativos.

### 4.2. Ataques de Redirección y Relaying (NTLM Relay & Responder)
* **Flujo de Intercepción NTLM:**
  1. Detención de servicios HTTP/SMB nativos si interfieren y ejecución de `Responder`.
  2. Configuración y ejecución de `ntlmrelayx.py`.
  3. Inducción o simulación de tráfico/búsquedas hacia recursos inexistentes para interceptar el hash NTLM de usuarios con privilegios elevados, permitiendo la ejecución de comandos o el volcado de bases de datos SAM de forma automatizada.

### 4.3. Técnicas de Enumeración de Dominio e Impacket
* **Herramientas de Reconocimiento de Directorio Activo:**
  * Uso de `rpcclient` para consultas detalladas de usuarios y grupos de seguridad (`querygroup`).
  * Conexión remota interactiva mediante `evil-winrm` a través de los puertos de WinRM (`5985`, `5986`).
* **Impacket Suite:**
  * `psexec.py` / `smbexec.py`: Ejecución de comandos remotos y obtención de shells interactivas utilizando credenciales o hashes válidos.
  * `GetNPUsers.py`: Búsqueda y extracción de hashes de usuarios configurados con la opción *"Do not require Kerberos preauthentication"* (AS-REP Roasting).
  * `GetUserSPNs.py`: Identificación de cuentas con Service Principal Names (SPNs) vulnerables a ataques de **Kerberoasting**, permitiendo guardar los tickets para su posterior descifrado offline con `hashcat` o `john`.
  * `secretsdump.py`: Volcado completo de hashes de contraseñas de la base de datos de Active Directory (NTDS.dit).

---
*Apuntes estructurados y listos para la integración continua en tu repositorio de GitHub y portafolio técnico.*
