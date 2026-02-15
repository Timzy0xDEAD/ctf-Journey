# TryHackMe – Vulnversity Write-up

### Nmap Scan
```bash
 nmap -sV <IP address>
 ```

![alt text](image.png)

### Directory enumeration 
```bash
 gobuster dir -u http://<IP address> -w /usr/share/wordlists/dirb/common.txt
``` 
![alt text](image-1.png)
### Web Inspection
```bash
 firefox http://10.49.167.250:3333/internal/    
```
![alt text](image-2.png)
### fuzz the upload form
 the wordlist: 
```bash
 echo -e "php\nphp3\nphp4\nphp5\nphtml" > extensions.txt
 ```
