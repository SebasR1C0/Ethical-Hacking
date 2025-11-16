# Criticalops
After my reconnaissance, when I created an account, I had access to all the tickets linked to my user. This led me to suspect that, if I could escalate my privileges to admin, I might be able to view private information belonging to other users.
<img width="914" height="272" alt="image" src="https://github.com/user-attachments/assets/1cf85af6-243b-4c62-90f2-a0fcd921f437" />

Another aspect that caught my attention was the JWT configuration shown here.
<img width="506" height="478" alt="image" src="https://github.com/user-attachments/assets/132a28ee-168b-42e8-9663-80262846fd47" />

I noticed that the algorithm used was HS256. I tried the common none attack and attempted to modify the role to admin, but the token was correctly rejected. While exploring the Debugger section, I found something interesting: Webpack debugging was enabled.
<img width="385" height="271" alt="image" src="https://github.com/user-attachments/assets/87d67358-cd2b-459f-9d76-1c5f3262212f" />

This provided extensive information about how the web application operates. By navigating through the Webpack bundle, I eventually found a hard-coded JWT secret key.
<img width="899" height="251" alt="image" src="https://github.com/user-attachments/assets/e2bef4ca-eec3-4a3a-b1a5-4a69b4cd73a7" />

Once I discovered the secret key, I created a new signature using HS256, since this algorithm relies on a symmetric key.
<img width="654" height="481" alt="image" src="https://github.com/user-attachments/assets/a492d5b3-51f7-4ca8-94fb-f17ea2da928f" />

With this, I simply modified my role to admin and replaced the original signature with the forged one.
<img width="1001" height="457" alt="image" src="https://github.com/user-attachments/assets/6bac5050-4381-4f5d-a80b-35d311bc20d1" />
