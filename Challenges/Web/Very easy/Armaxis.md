# Armaxis

At the beginning of the challenge, I noticed that the password-reset mechanism did not validate whether the reset token actually belonged to the requesting user. This meant that any valid reset token could be used to reset the password of any account. Leveraging this flaw, I reset the password for:
<img width="719" height="338" alt="image" src="https://github.com/user-attachments/assets/4056454f-af4c-483b-976d-e1c40b7fe847" />

After gaining access, I explored the “Dispatch Weapon” page, which contained multiple input fields that were reflected or processed by the backend. One field in particular drew my attention: the note field. When reviewing the backend logic, I observed that the server parsed Markdown input, extracted image URLs, and executed a shell command using curl -s:
<img width="915" height="304" alt="image" src="https://github.com/user-attachments/assets/0a8f38e4-51af-4d38-bfaa-98e7a968fdde" />

Because the URL originated directly from the Markdown image tag, it could be manipulated to inject arbitrary shell commands by breaking out of the intended syntax. To test this, I sent the following payload to test@email.htb:
```bash
![x](http://example.com; cat /flag.txt;
```
The server executed the injected command and returned an image containing base64-encoded output. After decoding the received base64 data, I recovered the contents of /flag.txt, successfully obtaining the flag.
