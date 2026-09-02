# OverTheWire: Bandit Level 24 - 25
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty

## The Challenge
"Making someone do the hard part for us?" Is how I would describe this level. There is a server at port 30002 in localhost, where you have to pass in the password of the current level and a 4-digit pin code. However, there is no other way to find the 4-digit pin code, except by trying out the 10,000 possible combinations. This method was noted as brute forcing.

## My Approach
In this situation, what I like to do is checking out if there are any possible clues with the server. I started by using `nmap` with the extra flag of `-sV` to check if there is any *SSL/TLS* encryption and to see if there's any other devices connected to the server.

> I know it wasn't specified, but I like checking it just to be sure.

The results came out like this:

```
Starting Nmap 7.98 ( https://nmap.org ) at 2026-09-02 15:43 +0000
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000024s latency).
Other addresses for localhost (not scanned): ::1

PORT      STATE SERVICE         VERSION
30002/tcp open  pago-services2?
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port30002-TCP:V=7.98%I=7%D=9/2%Time=6A98443A%P=x86_64-pc-linux-gnu%r(NU
SF:LL,97,"I\x20am\x20the\x20pincode\x20checker\x20for\x20user\x20bandit25\
SF:.\x20Please\x20enter\x20the\x20password\x20for\x20user\x20bandit24\x20a
SF:nd\x20the\x20secret\x20pincode\x20on\x20a\x20single\x20line,\x20separat
SF:ed\x20by\x20a\x20space\.\n")%r(GenericLines,129,"I\x20am\x20the\x20pinc
SF:ode\x20checker\x20for\x20user\x20bandit25\.\x20Please\x20enter\x20the\x
SF:20password\x20for\x20user\x20bandit24\x20and\x20the\x20secret\x20pincod
SF:e\x20on\x20a\x20single\x20line,\x20separated\x20by\x20a\x20space\.\nWro
SF:ng!\x20Please\x20enter\x20the\x20correct\x20current\x20password\x20and\
SF:x20pincode\.\x20Try\x20again\.\nWrong!\x20Please\x20enter\x20the\x20cor
SF:rect\x20current\x20password\x20and\x20pincode\.\x20Try\x20again\.\n")%r
SF:(GetRequest,129,"I\x20am\x20the\x20pincode\x20checker\x20for\x20user\x2
SF:0bandit25\.\x20Please\x20enter\x20the\x20password\x20for\x20user\x20ban
SF:dit24\x20and\x20the\x20secret\x20pincode\x20on\x20a\x20single\x20line,\
SF:x20separated\x20by\x20a\x20space\.\nWrong!\x20Please\x20enter\x20the\x2
SF:0correct\x20current\x20password\x20and\x20pincode\.\x20Try\x20again\.\n
SF:Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\x20a
SF:nd\x20pincode\.\x20Try\x20again\.\n")%r(HTTPOptions,129,"I\x20am\x20the
SF:\x20pincode\x20checker\x20for\x20user\x20bandit25\.\x20Please\x20enter\
SF:x20the\x20password\x20for\x20user\x20bandit24\x20and\x20the\x20secret\x
SF:20pincode\x20on\x20a\x20single\x20line,\x20separated\x20by\x20a\x20spac
SF:e\.\nWrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password
SF:\x20and\x20pincode\.\x20Try\x20again\.\nWrong!\x20Please\x20enter\x20th
SF:e\x20correct\x20current\x20password\x20and\x20pincode\.\x20Try\x20again
SF:\.\n")%r(RTSPRequest,129,"I\x20am\x20the\x20pincode\x20checker\x20for\x
SF:20user\x20bandit25\.\x20Please\x20enter\x20the\x20password\x20for\x20us
SF:er\x20bandit24\x20and\x20the\x20secret\x20pincode\x20on\x20a\x20single\
SF:x20line,\x20separated\x20by\x20a\x20space\.\nWrong!\x20Please\x20enter\
SF:x20the\x20correct\x20current\x20password\x20and\x20pincode\.\x20Try\x20
SF:again\.\nWrong!\x20Please\x20enter\x20the\x20correct\x20current\x20pass
SF:word\x20and\x20pincode\.\x20Try\x20again\.\n");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 157.40 seconds
```

### First attempt 

Reading the challenge description, I can assume that you have to make a script, so I started by doing `touch script.sh` to create a script. However, it came up with an error.

```
bandit24@bandit:~$ touch script.sh
touch: cannot touch 'script.sh': Permission denied
```

Which made me confused, so I checked the permissions of home directory if there was anything different by using `ls -la`, but nothing unusual came up.

This made me assume that I can't make scripts at home directory, so I created a temporary directory using `mktemp -d` and tested it again using `touch script.sh`, and worked normally.

Here, I started thinking how to make two scripts. One that would generate a list of all possible combinations for a 4-digit pin code and another that can input those combinations, along with the password of `bandit24` into the given server.

So, I did research on how to create shell scripts and came up with these for the scripts:

*pincode.script*
```
#!/bin/bash

echo "Generating 4-digit PIN wordlist..."
for pin in {0000..9999}; do
    echo "$pin" >> pin_wordlist.txt
done
echo "Wordlist saved to pin_wordlist.txt"

#!/bin/bash -x

PINCODES=pin_wordlist.txt
```
Script Breakdown:

- `#!/bin/bash` -> At first, I thought it was a comment, but it's actually telling the script to use the Bash/Shell Interpreter

- `echo ".."` - It's just for confirmation that the program started and ended

```
for pin in {0000..9999}; do
    echo "$pin" >> pin_wordlist.txt 
done
```

- This line is saying, "for each pin in the range of 0 - 9999, place each created pin in a word file called pin_wordlist.txt"


*script.sh*
```
#!/bin/bash -x

PINCODES=pin_wordlist.txt

if nc -z -w1 localhost 30002; then
    while IFS= read -r line || [[ -n "$line" ]]; do
        PASS="hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv"
        ATTEMPT="${PASS} $line"
        echo "$ATTEMPT"
        
        sleep 2
    done < "$PINCODES" | nc localhost 30002
else
    echo "Didn't Work"
fi

```



### Result

### Ruling things out

### The actual insight:

### Getting to the solution:

### The Solution

### Key Takeaway

### Tools/Commands Referenced


[Optional: link back to a running index / previous write-up / next write-up]
