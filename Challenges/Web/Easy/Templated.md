Al ingresar al reto vemos que nos indica un template conocido basado en python, que nos da una pista de ecnontrar una vuln de SSTI
<img width="518" height="264" alt="image" src="https://github.com/user-attachments/assets/beb50c2c-2848-4dcf-b40c-af5a0f2b8303" />

Al buscar en el directorio vemos que se visualiza la renderización en pantalla
<img width="518" height="273" alt="image" src="https://github.com/user-attachments/assets/98e27c32-fac1-4617-ac11-a0423d7bc587" />

Intentamos realizar una prueba conocida con {{7*7}} para ver si nos da 49 y confirmar la vuln
<img width="1117" height="407" alt="image" src="https://github.com/user-attachments/assets/523b9123-33a0-480a-bb2d-819260f7ae09" />

Una vez confirmada encontramos un payload en el repositorio de [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Python.md#jinja2)
<img width="1059" height="306" alt="image" src="https://github.com/user-attachments/assets/a6cfe9ef-5264-41c3-ab29-23f7d33bd6cd" />
