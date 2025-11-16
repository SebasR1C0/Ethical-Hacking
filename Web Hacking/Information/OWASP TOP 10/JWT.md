# JWT

| Tipo de Clave     | Algoritmos JWT Comunes        | Caso de Uso Principal                                      |
|-------------------|--------------------------------|-------------------------------------------------------------|
| **Symmetric Key** | HS256, HS384, HS512            | Sistemas internos donde se comparte una única clave secreta. |
| **RSA Key**       | RS256, PS256 *(recomendado)*   | APIs públicas y microservicios; permite verificación mediante clave pública. |
| **EC Key**        | ES256, ES384, ES512            | Entornos que requieren alta eficiencia y menor tamaño de clave. |
| **OKP**           | EdDSA                          | Alternativa moderna, segura y eficiente a las claves EC.    |



<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/7d2ae94f-851c-419a-92e8-babaaa454ff4" />
