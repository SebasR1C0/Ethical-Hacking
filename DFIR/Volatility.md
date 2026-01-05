# Ver árbol de dependencias
```
vol.exe -f .\MemoryDump.mem windows.pstree
```


# Ver permisos
```
vol.exe -f .\MemoryDump.mem windows.vadinfo --pid 5896
```

# Ver conexiones
```
vol.exe -f .\MemoryDump.mem windows.netscan | findstr "oneetx.exe"
```

# Detectar malware
```
vol.exe -f .\MemoryDump.mem windows.malfind.Malfind
```

# Encontrar Strings
```
strings.exe .\MemoryDump.mem | Select-string "77.91.124.20"
```

# Encontrar USB log
```
vol.exe -f .\MemoryDump.mem windows.registry.hivelist
# Offset de  \REGISTRY\MACHINE\SYSTEM

vol.exe -f .\MemoryDump.mem windows.registry.printkey --offset 0xbe8505256000 --key "MountedDevices"
vol.exe -f .\MemoryDump.mem windows.registry.printkey --offset 0xbe8505256000 --key "ControlSet001\Enum\USB"
vol.exe -f .\MemoryDump.mem windows.registry.printkey --offset 0xbe8505256000 --key "ControlSet001\Enum\USBSTOR"
```
