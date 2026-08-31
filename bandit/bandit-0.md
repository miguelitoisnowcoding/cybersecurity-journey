# OverTheWire: Bandit Level 0
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

> Miguel's Notes: As I'm writing this down, I'm already at bandit level 23. So, my memory on how I proceeded with the level will be quite inaccurate with how I really did it. So, I'm sorry in advance huhu

## The Challenge
*The first of many*, the level asks the user to utilize the command `ssh` to log in to the game. As seen at the top left corner of the OverTheWire website, the user must connect to `bandit.labs.overthewire.org` at port `2220` with the username and password of `bandit0`. The moment you're in, you can proceed to the level one page.

## My Approach

### First attempt 
Before I typed anything down, I looked into how to use the command `ssh` and I discovered that it followed this format: `ssh username@address -p portnumber`. 

So, using what I've learned, I tried to do it. I typed in `ssh bandit1.bandit.labs.overthewire.org -p 2220`

### Result
```
The authenticity of host '[bandit1.bandit.labs.overthewire.org]:2220 ([51.20.162.29]:2220)' can't be established.
ED25519 key fingerprint is: SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:1: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[bandit1.bandit.labs.overthewire.org]:2220' (ED25519) to the list of known hosts.
Connection closed by 51.20.162.29 port 2220
```

### Ruling things out
I was so confused. Why did it come out as an error? Did was there a typo? Did I format the command wrong? These were the questions I was asking to myself.

### The actual insight:
There I realized that I had a `.` instead of a `@`. 

### Getting to the solution:
So, I tried again, I replaced the `.` with a `@` in the `ssh` command
```
user@user:~$ ssh bandit0@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit0@bandit.labs.overthewire.org's password:

```
I entered the password, which was `bandit0`
```

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!
```

There it is! The welcome message that everyone loves.

### The Solution
`ssh bandit1@bandit.labs.overthewire.org -p 2220`

As shown in the manual, the `ssh` command is structured with the *username* followed by an `@` then the *address* of the server. With the flag/variable of `-p` or port succeeded by the actual port number, which is `2220` in this situation.

### Key Takeaway

The smallest mistakes can cause a significant change in the result of your commands. As seen how I typed in `.` instead of `@` between *username* and *serveraddress*, which caused me to connect to an unverified server. Therefore, the lesson here is to always check the syntax of your commands to ensure that it follows it's prescribed format.

### Tools/Commands Referenced

`ssh` - connects you to a server based on the *username*, *serveraddress*, and *portnumber*. Typically, it follows this format: `ssh username@serveraddress -p portnumber`

[Optional: link back to a running index / previous write-up / next write-up]
