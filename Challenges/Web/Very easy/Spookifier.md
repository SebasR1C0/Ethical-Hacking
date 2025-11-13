# Spookifier
At the beginning of the challenge, the application provided a single input field that transformed my text into other “spooky-themed” strings. Initially, I assumed the transformation engine might be vulnerable to Cross-Site Scripting (XSS). I attempted several payloads to extract cookies, but all attempts consistently failed due to strict output filtering.

<img width="671" height="625" alt="image" src="https://github.com/user-attachments/assets/76c2b50d-96a2-4392-8e12-8e2e0ab79c1c" />

SSince the XSS vector did not work, I shifted my focus to Server-Side Template Injection (SSTI). I tested the common expression ${7*7} to verify whether the input was processed by a template engine capable of evaluating expressions server-side. The output confirmed that code execution was happening
<img width="647" height="461" alt="image" src="https://github.com/user-attachments/assets/6b26149b-7e6f-48a7-b4f0-a5c8a2f90bf6" />

I had to identify the template in the web site, I identified a Mako template:
That's why I was searching payload for that [SSTI Payload](https://www.yeswehack.com/learn-bug-bounty/server-side-template-injection-exploitation)
I got a RCE attack 
<img width="481" height="185" alt="image" src="https://github.com/user-attachments/assets/af2cee88-48df-4a1a-a2e0-b2c515313590" />
So change the payload to "cat /falg.txt" and I got the flag!
```python
${self.module.cache.util.os.popen("cat /flag.txt").read()}
```
<img width="699" height="588" alt="image" src="https://github.com/user-attachments/assets/4902f28b-2987-4524-80ce-2bf1f9d593e3" />
