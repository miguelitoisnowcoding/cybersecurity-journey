# OverTheWire: Bandit Level ## - ##
Category: Linux Fundamentals | Date of Publish: 11/17/2026 | Difficulty: 

## The Challenge
The challenge instructs to pass along the password of the current level to a port from 31000-32000 hosted by localhost, however there is only one port that will send you the credentials of the next level. Hence, you have to identify which port is listening and which of them is encrypted with SSL/TLS.

## My Approach

### First Attempt
Since I'm dealing with a problem related to finding a port and identifying its details, and whether its listening or not, I used the command `nmap`. In order to get more information from the search that `nmap` provides, I used the plags -sV to display the services and the version the ports are using.

### Result

Which resulted into this:

```
bandit16@bandit:~$ nmap -sV -p 31000-32000 localhost
Starting Nmap 7.98 ( https://nmap.org ) at 2026-11-17 02:09 +0000
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00015s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.98%T=SSL%I=7%D=11/17%Time=6AFBB773%P=x86_64-pc-linux-
SF:gnu%r(GenericLines,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20c
SF:urrent\x20password\.\n")%r(GetRequest,32,"Wrong!\x20Please\x20enter\x20
SF:the\x20correct\x20current\x20password\.\n")%r(HTTPOptions,32,"Wrong!\x2
SF:0Please\x20enter\x20the\x20correct\x20current\x20password\.\n")%r(RTSPR
SF:equest,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20pa
SF:ssword\.\n")%r(Help,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20
SF:current\x20password\.\n")%r(FourOhFourRequest,32,"Wrong!\x20Please\x20e
SF:nter\x20the\x20correct\x20current\x20password\.\n")%r(LPDString,32,"Wro
SF:ng!\x20Please\x20enter\x20the\x20correct\x20current\x20password\.\n")%r
SF:(SIPOptions,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\
SF:x20password\.\n");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 122.74 seconds
```

### Getting to the solution:

With this output, I'm getting a lot of information on which port to use. As seen in the series of text under service, I can see that the `nmap` is telling me that port 31790 is displaying a series of text, specifically the text "Please enter the correct current password", which could mean that 31790 is the port I should use.

With this, I used `ncat` to connect to the port, with the added flag of `--ssl` since the port is encrypted with SSL/TLS. 

```
bandit16@bandit:~$ ncat --ssl localhost 31790
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
Correct!
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```

However, this isn't a password. Rather, its a key that I can use to access bandit level 17, so I have to find a way to convert this into a sshkey.

First, I have to redirect the output of `ncat` into a .private file, which I can do by making a pipeline consisting of an `echo` that passes along the password into `ncat` which connects to the server, then it redirects that output into a file.

```
bandit16@bandit:~$ echo "kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V" | ncat --ssl localhost 31790 > sshkey.private
-bash: sshkey.private: Permission denied
```

It didn't work, which meant I have the permissions to do it in this directory, so I made a temporary directory using `mktemp -d`

```
bandit16@bandit:~$ mktemp -d
/tmp/tmp.xfnCnr0ynH
bandit16@bandit:~$ cd /tmp/tmp.xfnCnr0ynH
bandit16@bandit:/tmp/tmp.xfnCnr0ynH$
bandit16@bandit:/tmp/tmp.xfnCnr0ynH$ echo "kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V" | ncat --ssl localhost 31790 > sshkey.private
bandit16@bandit:/tmp/tmp.xfnCnr0ynH$ ls
sshkey.private 
```

There, now I just need to transfer it to my personal directory and edit it. In order to transfer it, I used `scp` and directed it to bandit16 with it's path being the file location of `sshkey.private`. Importantly, I ended the prompt with `.` to ensure that it's saved in my directory.

```
migz@Miguel:~$ scp -P 2220 bandit16@bandit.labs.overthewire.org:/tmp/tmp.xfnCnr0ynH/sshkey.private .
```

Afterwards, I edited it using `nano` because previously seen, before the `----BEGIN OPENSSH ...` segment, there was a line "Correct!". So, I deleted it using with nano.

### The Solution

```
migz@Miguel:~$ nano sshkey.private
migz@Miguel:~$ ssh -i sshkey.private bandit17@bandit.labs.overthewire.org -p 2220
```

<-- [Previous Write-Up](bandit-15-16.md) | [Next Write-Up](bandit-17-18.md) -->
