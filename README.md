# LazyAdmin (CTF)

```
export ip=10.10.57.246
view the all scan results within the scan folder uploaded
```

# 1. What is the user flag?

```
always scanning the machine with nmap first
```
```json
nmap -sC -sV -oN scans/normal_nmap $ip
```
```
looks like ssh (OpenSSH 7.2p2) is open on port 22
as well as apache (Apache httpd 2.4.18) on port 80
so lets check out the web server
```
```
looks like its a default page so im going to run gobuster to find any hidden pages. All we found was /content/ 
which is quite useless to us however i ran http://10.10.57.246/content/ in gobuster and found 2 usefull sites. http://10.10.57.246/content/as/ which is a login and http://10.10.57.246/content/inc/ which a mysqal backup database.
```
```
this database had a username and a hash which i cracked with hashcat 
user = manager
password hash = 42f749ade7f9e195bf475f37a44cafcb
```
```json
hashcat -m 0 -a 0 -o cracked.txt 42f749ade7f9e195bf475f37a44cafcb /usr/share/wordlists/rockyou.txt
```
```
password is "Password123"
using these credientials im able to login into the dashboard
```
```
After searching around on the dashboard, I found "Media Center" which allows us to upload files. Abusing this vulernability I uploaded a php reverse shell.
```
```json
nc -lvnp "YOURPORT"
```
```
now by clicking onto file from the media center we are able to obtain a shell. First thing i always do when obtaining one is to upgrade the shell. I can do this by checking if python is installed.
```
```json
which python

"/usr/bin/python"
//response

python -c 'import pty;pty.spawn("bin/bash")'
//upgrade shell

ctrl + z
stty raw -echo
fg
```
```
now that we have a shell, the user flag is most often located either in the spawn location or in the home directory
```
```
Answer: THM{63e5bce9271952aad1113b6f1ac28a07}
```

# 2. What is the root flag?

```
The first step I like to do when trying to enerumerate is using the command
"sudo -l" This lists the permissions the user we are impersoanting has. 
```
```
Looks like theres a perl script that calls for another perl script in /etc/copy.sh 
that copy script has revershell code already in place for us, so we can just change the port and ip while setting up a listener in a new terminal.
```
```json
nc -lvnp "YOUR_PORT"
```
```
Now that we have root its as simple as going to the root directory and finding the flag. 
```
```
Answer: THM{6637f41d0177b6f37cb20d775124699f}
```
```
Congrats on completing the room! 
```
