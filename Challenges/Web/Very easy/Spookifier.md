# Spookifier
At the beggining, only I have a inputn that modify mi input to anothers string so I wass trying xss. I tried to get a cookie but it was impossible

<img width="671" height="625" alt="image" src="https://github.com/user-attachments/assets/76c2b50d-96a2-4392-8e12-8e2e0ab79c1c" />

So I changed the payload for Server-side Template Injection (SSTI): I tried ${7*7}
<img width="647" height="461" alt="image" src="https://github.com/user-attachments/assets/6b26149b-7e6f-48a7-b4f0-a5c8a2f90bf6" />

I had to identify the template in the web site, I identified a Mako template:
That's why I was searching payload for that [SSTI Payload](https://www.yeswehack.com/learn-bug-bounty/server-side-template-injection-exploitation)
