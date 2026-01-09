  # Etapas
  En las etapas de respuestas a incidentes
  1. Preparación: Definir los procesos y herramientas a utilizar 
  2. Identificación y detección: Detección del incidente, equipos afectados y determinar el alcance inicial
  3. Contención: Contener los equipos afectados para evitar su movimiento laterar y propagación del virus
  4. Erradicación: Eliminar el malware, persistencia y backdoors
  5. Recuperación: Restaurar los ervicios
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

# Adquisición de Disco (Triage vs. Imagen Completa)

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
- SECURE: Guarda las políticas de seguridad, auditoría y privilegios del sistema (LSA).
- NTUSER.DAT: Configuración específica de cada usuario (historial de búsqueda, archivos recientes).

# Consideraciones
## NTFS
Utilizado en windows para organizar, ordenar, almacenar y aisgnar los roles a cada archivo del disco duro

## ADS
Característica exclusiva de NFTS, sirve para ocultar información en un archivo

## Veracidad
Disco: Para identificar la integridad de la información recolectada validar la similitud de hash del archivo extraído
Disco Copiado: El Write Blocker es el mecanimo de la prohibición de edición en el archivo extraído en el proceso forense 

## Elección de .E01
Porque el E01 permite compresión (ahorra espacio), protección con contraseña y guarda metadatos del caso (hashes, nombre del perito) dentro del mismo archivo.
