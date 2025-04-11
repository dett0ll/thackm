nmap -p- -sV -n 10.10.204.129 -o nmap.txt  
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-04-11 12:22 EDT  
Nmap scan report for 10.10.204.129  
Host is up (0.17s latency).  
Not shown: 65534 closed tcp ports (reset)  
PORT   STATE SERVICE VERSION  
80/tcp open  http    PHP cli server 5.5 or later (PHP 8.1.0-dev)  

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 1226.40 seconds  

it used php 8.1.0  
https://www.exploit-db.com/exploits/49933  

run the exploit  

python3 49933.py                                       
Enter the full host url:  
http://10.10.204.129/  

find / flag.txt
