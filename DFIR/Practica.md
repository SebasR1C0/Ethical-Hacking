# Etapas
En las etapas de respuestas a incidentes
1. Preparación: Definir los procesos y herramientas a utilizar 
2. Identificación y detección: Detección del incidente, equipos afectados y determinar el alcance inicial
3. Contención: Contener los equipos afectados para evitar su movimiento lateral y propagación del virus
4. Erradicación: Eliminar el malware, persistencia y backdoors
5. Recuperación: Restaurar los servicios
6. Lecciones aprendidas: Información forense y corregir errores

# Etapas específicas de la Adquisición Forense
1. Planeación
2. Identifiación de evidencias
3. Adquisión
4. Preservación
5. Documentación y cadena de custodia

# Tipos de Adquisión
En la adquisión forense existen 2 tipos:
1. Equipo Activo: Equipo encendido y en ejecución
- RAM
- Procesos
- Conexiones de red
- Claves cifrado
- Malware en ejecución
2. Equipo Muerto: Equipo apagado
- Disco duro
- Log históricos
- Archivos persistentes

# La regla (RFC 3227) - Orden de volatilidad
1. Registros y caché: Se sobreescriben constantemente, se pierden al cerrar
2. Tablas de enrutamientos, caché arp, tabla de procesos y memoria: Solo existe cuando el equipo esta encendido
3. Archivos temporales.
4. Disco.
5. Ficheros de logs.
6. Configuración física, topología de red: Cambia lentamente
7. Almacenamiento externo.
El orden puede adaptarse según el impacto al negocio y los objetivos del cliente.

# Cadena de custodia
- Quién
- Cuándo
- Dónde
- Cómo se manipuló la evidencia

# Reporte forense
- Cronología
- Evidencia
- Impacto
- Recomendaciones

# Adquisición de Memoria RAM (Live Response)
La adquisición por memoria se debe realizar conectando un usb para no comprometer la integridad de los datos.
- Hiberfil.sys: Archivo de hibernación. Cuando Windows hiberna, copia la RAM al disco duro.
- Pagefile.sys: Archivo de paginación. Windows mueve partes de la RAM aquí cuando se queda sin memoria física.

# Adquisición de Disco (Triage vs. Imagen Completa)
1. Copia Completa: Aquí residen los archivos borrados que aún no han sido sobrescritos. Solo una imagen completa permite la recuperación de datos eliminados (Data Carving).
2. Triage Forense (Adquisición Selectiva / Fast Forensics)
KAPE

# Artefactos de Ejecución (Evidence of Execution)
"¿Se ejecutó este malware/programa en la máquina?"
- Prefetch (.pf): Windows crea estos archivos para acelerar el inicio de aplicaciones. Dicen el nombre del ejecutable, cuántas veces se abrió y la fecha de la última ejecución. (Ruta: C:\Windows\Prefetch)
- Shimcache (AppCompatCache): Sirve para compatibilidad de aplicaciones, pero guarda un historial de ejecutables, incluso si el archivo original ya fue borrado.
- AmCache: Similar al Shimcache, pero guarda el hash SHA-1 del ejecutable.

Busco evidencia de ejecución en artefactos como Prefetch, Shimcache y AmCache. Prefetch me indica cuándo se ejecutó y cuántas veces; Shimcache y AmCache permiten detectar ejecutables incluso si ya fueron eliminados

# Artefactos de Acceso a Archivos y Carpetas
"¿El usuario sabía que ese archivo estaba ahí? ¿Lo abrió?"
- LNK Files (Accesos directos): Cuando abres un archivo, Windows crea un .lnk en "Recientes". Te dice la ruta original y fechas, incluso si el archivo estaba en un USB que ya no está conectado.
- Shellbags: Son claves de registro que guardan el tamaño y posición de las ventanas del explorador. Si un atacante navegó por carpetas buscando información, los Shellbags te dirán qué carpetas visitó, incluso si esas carpetas ya no existen.
- JumpLists: Las listas de "recientes" que aparecen al dar clic derecho en un icono de la barra de tareas.

# La MFT (Master File Table)
- Si un archivo es muy pequeño (aprox. menos de 700 bytes), NTFS no lo guarda en el disco normal, lo guarda directamente dentro de la MFT.
- Timestamps ($STANDARD_INFORMATION vs $FILE_NAME): Un archivo tiene dos sets de fechas. Los atacantes suelen modificar uno (Timestomping) para ocultarse, pero a veces se olvidan del otro. La MFT guarda ambos.

# Event Logs (Windows Event Logs) - Los IDs Clave
- 4624: Inicio de sesión exitoso (Logon Success).
- 4625: Inicio de sesión fallido (Logon Failure) -> Muchos de estos indican fuerza bruta.
- 4672: Inicio de sesión con privilegios de administrador (Special Privileges).
- 1102: El log de auditoría fue borrado (Audit Log Cleared) -> Señal casi segura de actividad maliciosa.

# Registro de Windows (The Registry)
- SAM: Guarda los hashes de las contraseñas de usuarios locales.
- SYSTEM: Configuración del sistema, zona horaria y USBSTOR (historial de USBs conectados).
- SOFTWARE: Programas instalados y versiones.
- SECURITY: Guarda las políticas de seguridad, auditoría y privilegios del sistema (LSA).
- NTUSER.DAT: Configuración específica de cada usuario (historial de búsqueda, archivos recientes).

# Rutas
## 1. Actividad del Usuario (User Activity)
Evidencia de qué archivos abrió el usuario y qué programas ejecutó.

### UserAssist
* **Ruta:** `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist`
* **Función:** Rastrea la ejecución de programas con interfaz gráfica (GUI) por usuario.

### RecentDocs
* **Ruta:** `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`
* **Función:** Muestra la interacción con archivos recientes (los últimos abiertos o guardados).

### ShellBags
* **Ruta:** `HKCU\SOFTWARE\Classes\Local Settings\Software\Microsoft\Windows\Shell\BagMRU` (y `\Bags`)
* **Función:** Rastrea la navegación de carpetas del explorador por usuario. [cite_start]Útil para saber qué carpetas visitó[cite: 25, 26].

### RunMRU
* **Ruta:** `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`
* [cite_start]**Función:** Historial de comandos escritos en la ventana "Ejecutar" (Start -> Run)[cite: 8, 17].

### TypedPaths
* **Ruta:** `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths`
* [cite_start]**Función:** Rutas ingresadas manualmente en la barra de direcciones del Explorador[cite: 9, 18].

---

## 2. Dispositivos USB y Almacenamiento Externo (USB Forensics)
Crucial para investigar exfiltración de datos o infecciones físicas.

### USBSTOR
* **Ruta:** `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR`
* [cite_start]**Función:** Contiene información del dispositivo: ID del vendedor (VID), ID del producto (PID) y Número de Serie[cite: 38, 39].

### Mounted Devices
* **Ruta:** `HKLM\SYSTEM\Mounted Devices`
* [cite_start]**Función:** Permite encontrar la letra de la unidad (ej: `E:`) asociada al número de serie del USB[cite: 53, 54].

### MountPoints2
* **Ruta:** `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Mountpoints2`
* [cite_start]**Función:** Permite identificar al **usuario específico** que montó/conectó el dispositivo USB[cite: 67, 68].

---

## 3. Persistencia (Persistence)
Lugares donde el malware se configura para iniciar automáticamente con Windows.

### Run / RunOnce (Usuario Actual)
* **Ruta:** `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
* [cite_start]**Función:** Inicia programas automáticamente al iniciar sesión ese usuario específico[cite: 30, 31].

### Run / RunOnce (Todo el Sistema)
* **Ruta:** `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
* [cite_start]**Función:** Inicia programas automáticamente para **todos** los usuarios[cite: 32, 33].

---

## 4. Ejecución del Sistema (System Execution)
Evidencia técnica de que un programa existió en el disco.

### AppCompatCache (Shimcache)
* **Ruta:** `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`
* **Función:** Conocido como "Shimcache". Guarda la ruta completa del archivo, nombre y fecha de última modificación. [cite_start]Puede probar la ejecución incluso si el archivo ya fue borrado[cite: 111, 112, 115].

---

## 5. Redes y Conexiones (Network & RDP)
Historial de conexiones de red y accesos remotos.

### NetworkList (Perfiles de Red)
* **Ruta:** `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList` (Subclaves: `Signatures` y `Profiles`)
* **Función:** Evidencia de cada red a la que se ha conectado la máquina. [cite_start]Permite ver la fecha de la primera y última conexión a esa red[cite: 90, 88, 89].

### Terminal Server Client (RDP)
* **Ruta:** `HKCU\SOFTWARE\Microsoft\Terminal Server Client\Servers`
* [cite_start]**Función:** Rastrea los objetivos (IPs o nombres de dominio) a los que el usuario se conectó mediante Escritorio Remoto (RDP)[cite: 19, 20].

### Interfaces de Red
* **Ruta:** `HKLM\SYSTEM\CurrentControlSet\services\Tcpip\Parameters\Interfaces`
* [cite_start]**Función:** Almacena la configuración de las interfaces de red (IPs, máscaras, etc.)[cite: 84, 85].

---

## 6. Información General del Sistema
Contexto básico de la máquina.

### Nombre del Equipo
* [cite_start]**Ruta:** `HKLM\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName` [cite: 81]

### Zona Horaria
* [cite_start]**Ruta:** `HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation` [cite: 80]

# Consideraciones
## NTFS
Utilizado en windows para organizar, ordenar, almacenar y aisgnar los roles a cada archivo del disco duro

## ADS
Característica exclusiva de NTFS, sirve para ocultar información en un archivo

## Veracidad
Disco: Para identificar la integridad de la información recolectada validar la similitud de hash del archivo extraído
Disco Copiado: El Write Blocker es el mecanimo de la prohibición de edición en el archivo extraído en el proceso forense 

## Elección de .E01
Porque el E01 permite compresión (ahorra espacio), protección con contraseña y guarda metadatos del caso (hashes, nombre del perito) dentro del mismo archivo.
