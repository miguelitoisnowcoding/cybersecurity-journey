# OverTheWire: Bandit Level 32 - 33
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

## The Challenge
A different challenge this time, the user is placed in a level where whatever they type is outputted as an uppercase, which the terminal doesn't understand.

## My Approach

### First Attempt
At first, I was really confused. Whatever command I typed in, was uppercase. I tried using `ls` to check and `cd` to see if they would work, but the commands were uppercased.

```
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: Permission denied
>> cd
sh: 1: CD: Permission denied
>>
```

So, I did research. I had to find a way to input a command that would either interpret my input as a lowercase or a command that would stop the script turning whatever I say into uppercase.

### Result

After research, I discovered that you can bypass the uppercase script by passing or substituting a variable. Apparently, the variable that I would need in this situation would be `$0` because of how bash interprets it.

I had to understand that there's a concept called variable expansion and it's the reason why I need it in this situation. `$0` holds the value of the current file path of shell and when you prompt it into the uppercase terminal, rather than getting uppercased, Bash finds or expands the file to see it's value.

The value of `$0` being the file path of shell, because of this value, it initiates a new unrestricted program to take over and to make the user interact to. This new program doesn't have the uppercase filter, thus allowing us to enter any commands.

```
WELCOME TO THE UPPERCASE SHELL
>> $0
$
```

### Getting to the solution:
When I arrived in this unrestricted terminal, I noticed one thing, the username isn't being displayed. This made question what user I was and what directory I was in, so I checked using `whoami` and `pwd`

```
WELCOME TO THE UPPERCASE SHELL
>> $0
$ whoami
bandit33
$ pwd
/home/bandit32
$
```

There I saw that I am under the user `bandit33`, but I was in the directory of `bandit32`. This is good because it means I can access the `/etc/bandit_pass` directory and find the password of `bandit33` there by using `cat`

### The Solution

```
$ cat /etc/bandit_pass/bandit33
u4P2CyPOwPGLe94RdD9Uo2FxFwvnFswM
```

### Key Takeaway
This was a level where I learned two concepts because of the research I did, *variable expansion* which is when a variable is passed and bash expands it to reveal another value and *command substitution* where we take the value of a command and use it as a parameter for a command.


<-- [Previous Write-Up](bandit-31-32.md) | [Next Write-Up](bandit-32-33.md) -->
