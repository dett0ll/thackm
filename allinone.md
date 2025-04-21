### NMAP  
nmap -sV 10.10.197.5 -o nmap.txt  
port -> 21,22,80  
ffuf -u http://10.10.197.5/FUZZ -w /usr/share/wordlists/dirb/common.txt  
http://10.10.197.5/wordpress/    
sudo gem install wpscan    
