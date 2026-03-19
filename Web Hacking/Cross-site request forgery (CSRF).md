# Cross-site request forgery (CSRF)
La vulnerabilidad es que hace hace creer al sistema que el usuario esta haciendo peticiones, como realizar transferencias bancarias o cambiar de contraseña

La manera en que podemos usar esto es con burp -> seleccionamos una peticion -> engagement tools -> generate CSRF

# Bypassing CSRF token validation
Casos de vulneracion:
1. Cambiar el metodo de Get a Post o viceversa
2. Eliminado el parametro del CSRF token
3. No valida si el token es es de la sesion
4. Los token no validan similitud entre ellos
5. El token es igual a la cookie
- Payload
Espera que abra la imagen para que mande a correr el demas codigo
```
<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>
```
