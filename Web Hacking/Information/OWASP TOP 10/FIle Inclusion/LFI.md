## Basic LFI
<img width="1582" height="685" alt="image" src="https://github.com/user-attachments/assets/4e2ac86e-c3f5-4ea0-bc0f-03fd437947c8" />

## Path Traversal
<img width="1580" height="685" alt="image" src="https://github.com/user-attachments/assets/900d504c-28a7-4837-b6a5-54807a7c9455" />

## Filename Prefix
With the use of prefixes
```bash
include("lang_" . $_GET['language']);
```
<img width="1395" height="261" alt="image" src="https://github.com/user-attachments/assets/7bba19ec-c4c5-4f36-84b8-0c55673844db" />

We have to start with "/"
<img width="1398" height="352" alt="image" src="https://github.com/user-attachments/assets/a637d1c9-ce2c-408e-9b77-c073b74bb493" />

## Appended Extensions
With the use of extension
```bash
include($_GET['language'] . ".php");
```
<img width="1582" height="509" alt="image" src="https://github.com/user-attachments/assets/3d57c0a6-97ff-4afe-94ca-c574a1b0141c" />

Note: We can bypass this with injection commands
## Second-Order Attack
For example, a web application may allow us to download our avatar through a URL like (/profile/$username/avatar.png). If we craft a malicious LFI username (e.g. ../../../etc/passwd), then it may be possible to change the file being pulled to another local file on the server and grab it instead of our avatar.

In this case, we would be poisoning a database entry with a malicious LFI payload in our username. Then, another web application functionality would utilize this poisoned entry to perform our attack (i.e. download our avatar based on username value). This is why this attack is called a Second-Order attack.
