# Request Models
- GET
- POST
- HEAD: Request Header
- PUT
- DELETE
- OPTIONS
- PATH

# Differente ways to delete a file
```bash
# Parameter
DELETE /admin/reset.php?file=notes.txt 
# Api
DELETE /admin/reset.php/dsdadsad HTTP/1.1
# JSON
DELETE /admin/reset.php HTTP/1.1
Content-Type: application/json 
{"file":"chupapi"}

```
# Changing the request method in Burp
Right click + CHange he request method 
<img width="681" height="252" alt="image" src="https://github.com/user-attachments/assets/39da71a6-5d80-469e-9e93-74e79b07a1f2" />
