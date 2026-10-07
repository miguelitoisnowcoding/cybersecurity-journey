# OverTheWire: Bandit Level 14 - 15
Category: Linux Fundamentals | Date of Publish: 10/07/2026 | Difficulty: Easy

## The Challenge
The challenge of this level is that the password of the next level is hidden, but you can uncover it by sending the password of the current level to a server in localhost at port 30000.

## My Approach

### First Attempt
The first thing I had to consider before doing anything else was to find out what the password of the current level was. Since, I used an ssh key to access `bandit 14`, I don't know the actual password of the current level.

However, I noticed in the previous bandit challenge description that the password is stored in `/etc/bandit_pass/bandit14` and since we are in `bandit 14`, we can access it with no problem.

### Result
```
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```

### Getting to the solution:
After finding out what the password of the next level was, I needed to find a command that is able to safely connect to this remote server at port 30000, while also being able to send the password of the current level to it.

So, I went through the suggested list of commands and saw that the command `nc` or `netcat`, which is a general-purpose tool to either access or receive network connection. With that, I pipelined it with the `echo` command so that it passes along the password when I connect to the remote server at port 30000.

### The Solution
```
bandit14@bandit:~$ echo aaWecNkG4FhxJQxz07uiwzVP6bJiYS65 | nc localhost 30000
Correct!
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```

### Key Takeaway
I find it interesting that I'm approaching levels that require going through another server and passing along a value to receive another value.

### Tools/Commands Referenced
`nc` - also known as netcat, which allows the user to either make network connection or receive a raw network connection.

<-- [Previous Write-Up](bandit-13-14.md) | [Next Write-Up](bandit-15-16.md) -->
