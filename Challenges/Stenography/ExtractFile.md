# Extract File
- Confirm
```bash
steghide info archivo.jpg
```

- Brute Force
```bash
stegcracker perritopoton.jpg /usr/share/wordlists/rockyou.txt
```

```bash
sebastianrojas@sebas:~/Descargas/reto_perritopoton$ binwalk perritopoton.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
89196         0x15C6C         Zip archive data, at least v2.0 to extract, compressed size: 1306, uncompressed size: 4282, name: ogidocledortnelodlacsub.py
90586         0x161DA         Zip archive data, at least v2.0 to extract, compressed size: 23, uncompressed size: 32, name: pista.txt
90851         0x162E3         End of Zip archive, footer length: 22

sebastianrojas@sebas:~/Descargas/reto_perritopoton$ binwalk --extract perritopoton.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
89196         0x15C6C         Zip archive data, at least v2.0 to extract, compressed size: 1306, uncompressed size: 4282, name: ogidocledortnelodlacsub.py
90586         0x161DA         Zip archive data, at least v2.0 to extract, compressed size: 23, uncompressed size: 32, name: pista.txt

WARNING: One or more files failed to extract: either no utility was found or it's unimplemented

sebastianrojas@sebas:~/Descargas/reto_perritopoton$ steghide extract -sf perritopoton.jpg -p ROMPECABEZAS
anot� los datos extra�dos e/"flag.txt".

```
