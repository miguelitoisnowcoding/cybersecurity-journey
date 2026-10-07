# OverTheWire: Bandit Level 15 - 16
Category: Linux Fundamentals | Date of Publish: 10/07/2026 | Difficulty: Easy

## The Challenge
The challenge of this level is similar to the one in the previous level, but what's changed is that the remote server at port 30001 is encrypted with SSL/TLS, so the user has to find a way to access the SSL/TLS encrypted server at port 30001 and pass along the password of the current level.

## My Approach

### Getting to the solution:
The whole process is very much similar to the process of the previous level, however, I need to find a command that will allow me to safely connect to the server that is SSL/TLS encrypted and pass along the password of the current level.

So, as usual, I went through the suggested commands of the level and discovered the command called `ncat` which is all-purpose tool that will allow users to do various things, including connecting and writing data to networks using various protocols and encryptions.

There, I pipelined `echo` and `ncat`, but one thing to take note is that I utilized the `--ssl` flag. The `--ssl` flag allows the ncat to access servers or ports that are encrypted with SSL.

### The Solution
```
bandit15@bandit:~$ echo pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7 | ncat --ssl localhost 30001
Correct!
kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V
```

### Key Takeaway
This is where I started discovering that I will be needing tools and commands that will help me access various servers that are encrypted, which I find very interesting as I go along my journey in cybersecurity.

### Tools/Commands Referenced
`ncat` - reads and writes data across networks using various protocols and encryptions.

<-- [Previous Write-Up](bandit-14-15.md) | [Next Write-Up](bandit-16-17.md) -->
