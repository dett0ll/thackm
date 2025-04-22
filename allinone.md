### NMAP  
nmap -sV 10.10.197.5 -o nmap.txt  
port -> 21,22,80  
ffuf -u http://10.10.197.5/FUZZ -w /usr/share/wordlists/dirb/common.txt  
http://10.10.197.5/wordpress/   
http://10.10.197.5/hackathon/  
sudo docker run -it --rm wpscanteam/wpscan --url http://10.10.197.5/wordpress/wp-login.php --enumerate u  
/hackathon-> view source  
http://10.10.55.80/wordpress/  
we find a cipher and key  
https://cryptii.com/pipes/vigenere-cipher   
we can see a blog by user elyana  
Login to http://10.10.55.80/wordpress/wp-login/  with user elyana  
http://10.10.55.80/wordpress/wp-admin/media-new.php  -> unable to upload any php file with modifications in extension  
http://10.10.55.80/wordpress/wp-content/themes/twentytwenty/style.css?ver=1.5  we found this link in source code  
http://10.10.55.80/wordpress/wp-admin/theme-editor.php?file=404.php&theme=twentytwenty  -> paste reverse shell here and then  
http://10.10.55.80/wordpress/wp-content/themes/twentytwenty/404.php  
start nc -lnvp port  
we get shell as www-data  
user.txt  permission denied  
find / -user elyana 2>/dev/null  
we get the path and save password  
ssh as user  
flag is base64 encoded  
sudo -l  
https://gtfobins.github.io/gtfobins/socat/   


 









