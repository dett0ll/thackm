port 80 -> ssh  
port 22 -> apache server  

http://10.10.16.234:22/ -> is a web server  
View page source -> /recovery.php and a base64 encoded value   
login page at recover.php  
base64 is decoded we get some pass phrase  
ffuf -u http://10.10.16.234:22/FUZZ -w /usr/share/wordlists/dirb/common.txt -o ffuf.txt  

