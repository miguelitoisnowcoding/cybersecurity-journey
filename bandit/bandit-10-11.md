# OverTheWire: Bandit Level 10-11
Category: Linux Fundamentals | Date of Publish: 10/06/2026 | Difficulty: Easy

## The Challenge
This next challenge revolves around a file called data.txt, that contains the password to the next level, which is encoded with base64 data. The user is tasked to decode the file to access the password.

## My Approach

### Getting to the solution:
Looking through the list of commands that the level suggests, I believe it was an obvious pick to use `base64` to decode a base64 encoded file. So I used `man base64` to look through the instructions and descriptions on how to use it. There, the command that I used was `base64 --decode data.txt`.

### The Solution
```
bandit10@bandit:~$ base64 --decode data.txt
The password is pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro
```

### Key Takeaway
I think this is where the importance of reading through the suggested commands is shown. I also find it interesting that I'm finally starting to go through levels that require decoding.

### Tools/Commands Referenced
`base64` - decodes base64 encoded files.

<-- [Previous Write-Up](bandit-09-10.md) | [Next Write-Up](bandit-11-12.md) -->
