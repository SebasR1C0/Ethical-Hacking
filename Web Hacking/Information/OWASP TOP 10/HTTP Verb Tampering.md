# Request Models
- GET
- POST
- HEAD: Request Header
- PUT
- DELETE
- OPTIONS
- PATH

Note: Differente ways to delete a file
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
