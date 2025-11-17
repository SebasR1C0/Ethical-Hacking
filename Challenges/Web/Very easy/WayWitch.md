# WayWitch
While reviewing the application’s source code, I noticed that the website relies on JSON Web Tokens (JWT) to manage user authentication and authorization. The most critical finding was that the secret phrase used to sign the JWTs was hard-coded in the backend:
<img width="581" height="316" alt="image" src="https://github.com/user-attachments/assets/3c43b991-17c7-4b6f-8a4f-e918c9661cc0" />

Additionally, I observed that viewing all created tickets required Administrator privileges. This authorization check was based solely on the value of the role field inside the JWT payload:
<img width="609" height="217" alt="image" src="https://github.com/user-attachments/assets/a6597afc-41a2-446e-966b-beb5cf9f72f0" />

Since the server’s secret key was publicly available in the source code, it was possible to forge a valid token. By generating a new JWT signed with the same secret and modifying the payload to:
<img width="937" height="378" alt="image" src="https://github.com/user-attachments/assets/96dd6cf9-7cd1-4d12-9070-4e5d77ad4465" />
