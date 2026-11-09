<div align="center">

# 🖥️ Trabajo Práctico Integrador (TPI) - Debian 12

<img width="1365" height="768" alt="image_42fa9190" src="https://github.com/user-attachments/assets/5cc2af05-1321-4aea-82b6-e208bd241773" />


[![Debian Version](https://img.shields.io/badge/OS-Debian%2012%20Bookworm-A80030?style=for-the-badge&logo=debian&logoColor=white)](https://www.debian.org/)
[![Bash Scripting](https://img.shields.io/badge/Shell-Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Cron Scheduled](https://img.shields.io/badge/Automation-Cron-00599C?style=for-the-badge&logo=clock&logoColor=white)](https://www.gnu.org/software/mcron/)
[![License](https://img.shields.io/badge/Academic-TPI-blue?style=for-the-badge)](https://www.palermo.edu/)

*Proyecto de configuración de almacenamiento, scripts de respaldo automatizado y estructuración de servidores Linux.*

---

</div>

## 👥 Integrantes del Grupo

| Nombre y Apellido | Carrera | GitHub / Contacto |
| :--- | :--- | :--- |
| **Claudio Ruben Arispe** | Lic. en Inteligencia Artificial | [@crapz0190](https://github.com/crapz0190) |
| **Nicolas Maximiliano Flores** | Carrera2 | [@usuario2](https://github.com/usuario2) |
| **Máximo Frías** | Carrera3 | [@usuario3](https://github.com/usuario3) |
| **Fiorela Sperani** | Carrera4 | [@usuario4](https://github.com/usuario4) |
| **Bruno Vallone** | Carrera5 | [@usuario5](https://github.com/usuario5) |

---

## 📌 Descripción General del Proyecto

Este repositorio documenta y almacena las evidencias técnicas correspondientes al desarrollo del **Trabajo Práctico Integrador (TPI)**. El proyecto abarca la configuración integral de una máquina virtualizada sobre **Debian 11 Bullseye**, estructurada bajo los siguientes módulos obligatorios:

1. **Configuración del Entorno:** Blanqueo de credenciales de administrador, establecimiento de contraseña segura (`palermo`) y configuración del hostname del sistema como `TPServer`.
2. **Servicios del Sistema:** Actualización total de la distribución a Debian 12, despliegue y aseguramiento de SSH (clave pública/privada para root), servidor web Apache con PHP para servir `index.php` y `logo.png`, y motor de base de datos MariaDB con la importación del script `db.sql`.
3. **Configuración de Red:** Establecimiento de direccionamiento IP estático especificando los campos `ADDRESS`, `NETMASK` y `GATEWAY` en el mismo segmento de red de la máquina física.
4. **Almacenamiento y Particionado:** Adición de un disco virtual secundario de 10 GB con particiones estándar de tipo 83 (`/www_dir` de 3 GB y `/backup_dir` de 6 GB), configuración de puntos de montaje automáticos en `/etc/fstab`, reubicación del sitio web y respaldo del mapeo de particiones en `/opt/particion`.
5. **Automatización de Respaldos:** Desarrollo del script en Bash `backup_full.sh` con validaciones de argumentos, control de disponibilidad de sistemas de archivos y calendarización automática mediante `cron`.
6. **Entregables y Control de Versiones:** Estructuración de directorios comprimidos individualmente en formato `.tar.gz` y fragmentación (*splitting*) del directorio `/var` para cumplir con los límites del repositorio.

---

## ⚙️ Detalle Técnico por Módulos

### Módulo 1: Configuración del Entorno
* **Virtualización:** Importación del entorno preconfigurado desde Blackboard en Oracle VirtualBox.
* **Credenciales y Hostname:** Restablecimiento de privilegios de `root` con contraseña definitiva `palermo` y definición del nombre de host a `TPServer`.

### Módulo 2: Servicios Base
* **Actualización del SO:** Migración y actualización completa de paquetes de la distribución a **Debian 12 Bookworm**.
* **Servicio SSH:** Habilitación del acceso remoto para el usuario administrador mediante autenticación por pares de claves criptográficas (disponibles originalmente en el directorio *Home* de root).
* **Servidor Web (Apache + PHP):** Instalación de Apache con soporte para PHP 7.3 o superior, configurado para servir los recursos gráficos y lógicos (`index.php` y `logo.png`) provistos en el entorno inicial.
* **Base de Datos (MariaDB):** Despliegue del motor gestor MariaDB y carga automatizada del esquema de datos mediante el script SQL `db.sql`.

### Módulo 3: Configuración de Red
* Establecimiento de una interfaz de red estática, declarando explícitamente los parámetros de red requeridos:
  * `ADDRESS`
  * `NETMASK`
  * `GATEWAY`

### Módulo 4: Esquema de Almacenamiento
* **Disco Adicional:** Conexión de un disco virtual de 10 GB.
* **Particiones Estándar (Tipo 83):**
  * `/www_dir`: Asignación de **3 GB** destinados al alojamiento de los archivos web del servidor Apache (`index.php` y `logo.png`) tras actualizar las directivas en `000-default.conf` y `apache2.conf`.
  * `/backup_dir`: Asignación de **6 GB** destinados a la persistencia de respaldos del sistema.
* **Montaje Automático:** Configuración persistente de particiones en `/etc/fstab`.
* **Registro de Particiones:** Volcado del contenido volátil de `/proc/partitions` hacia un archivo estático de control ubicado en `/opt/particion`.

### Módulo 5: Script de Respaldo Automatizado (`backup_full.sh`)
El script desarrollado y alojado en `/opt/scripts/` cumple con los siguientes estándares operativos:
* **Argumentos dinámicos:** Acepta parámetros de entrada estrictos para `<origen>` y `<destino>`.
* **Nomenclatura ANSI:** Genera nombres normalizados incluyendo la fecha del sistema en formato `YYYYMMDD` (ej. `logs_bkp_20240302.tar.gz`).
* **Validaciones de seguridad:** Cuenta con panel de ayuda interactivo (`-help`), control de cantidad exacta de parámetros y verificación de disponibilidad de los sistemas de archivos antes de operar.
* **Calendarización en Cron:**
  * **Diario (00:00 hs):** Respaldo automatizado de `/var/logs` hacia `/backup_dir`.
  * **Periódico (Lunes, Miércoles y Viernes a las 23:00 hs):** Respaldo automatizado de `/www_dir` hacia `/backup_dir`.

---
