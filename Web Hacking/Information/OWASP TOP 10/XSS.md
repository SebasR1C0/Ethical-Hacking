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
- <img src="" onerror=alert(window.origin)>
