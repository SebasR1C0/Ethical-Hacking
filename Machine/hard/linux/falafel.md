<img width="268" height="105" alt="image" src="https://github.com/user-attachments/assets/c7bad0e7-68de-4242-94b9-1ee8415af76f" />

IN this machine only there are 2 port open: shh y http

In the port 443: Only there is a login page and the first parameter is vuln (username) so I tried sqli
<img width="1033" height="275" alt="image" src="https://github.com/user-attachments/assets/bbd4eb22-4734-4088-b73d-228e15505c69" />

I entered like chris but there aren't options to find new vulns no I tries to entered in admin user, but I can't so surfing in the web I find this
This vuln is named Type Juggling in php, in a few words, the web page don't validate all string withe the differente vairables types, I could enter
<img width="1019" height="352" alt="image" src="https://github.com/user-attachments/assets/4034a381-bef0-4189-8ee8-32cf712b6bae" />

So I try to upload a file, but I can't upload a .php to try a reverse shell
SO in the profile site, I read "Know your limit" so I tried, put a big name file, and see that the web page has a limit to cut some word to save in the server
<img width="850" height="330" alt="image" src="https://github.com/user-attachments/assets/932fd852-4946-4a10-9626-d2e160eef604" />


With my revershell, I was in
<img width="1265" height="181" alt="image" src="https://github.com/user-attachments/assets/7037640f-6ade-4090-ba7f-9d8e5075e380" />

SO i try a lateral movement, and find private information of the moseh user 
<img width="577" height="293" alt="image" src="https://github.com/user-attachments/assets/d6253399-be32-4258-b4fa-8888dcf8231c" />

In dev path I find a fb0 that is a image, I extracted to my local and open it
<img width="513" height="277" alt="image" src="https://github.com/user-attachments/assets/72fdbee5-21af-4f7a-9729-0916e76f74fb" />

Now I'm yossi user and I part of disk groups so I search for privilege escalation
<img width="289" height="86" alt="image" src="https://github.com/user-attachments/assets/a0457855-a77e-4d61-b27e-c30e2c54654f" />

<img width="506" height="522" alt="image" src="https://github.com/user-attachments/assets/e84f28ae-4d09-45fd-83fa-c948c594661b" />

I log in like root
