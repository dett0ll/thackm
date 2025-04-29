port 80 -> ssh  
port 22 -> apache server  

http://10.10.16.234:22/ -> is a web server  
View page source -> /recovery.php and a base64 encoded value   
login page at recover.php  
base64 is decoded we get some pass phrase  
ffuf -u http://10.10.16.234:22/FUZZ -w /usr/share/wordlists/dirb/common.txt -o ffuf.txt  
/assets  
download all images using wget  
steghide --extract -sf header.jpg -> passphrase found above works  
we get username and password  
login to above web page  
we get this meesage "GET me a 'cmd' and I'll run it for you Future-Jack. "" this indicates it is prone to command injection  
http://10.10.16.234:22/nnxhweOV/index.php?cmd=whoami ->CI works  
Lets upload a php reverse shell  
we get shell as www-data  
in /home, we have jacks password list  
download to local server   
start a python server  
python -m SimpleHTTPServer  





