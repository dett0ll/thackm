### NMAP  
nmap -sV 10.10.197.5 -o nmap.txt  
port -> 21,22,80  
ffuf -u http://10.10.197.5/FUZZ -w /usr/share/wordlists/dirb/common.txt  
http://10.10.197.5/wordpress/   
http://10.10.197.5/hackathon/  
sudo docker run -it --rm wpscanteam/wpscan --url http://10.10.197.5/wordpress/wp-login.php --enumerate u
