# OverTheWire: Bandit Level 02 - 03
Category: Linux Fundamentals | Date of Publish: 09/15/2026 | Difficulty: Easy

## The Challenge
This challenge is similar to the last level where the user is challenged to access a file with an unusual file name. This time, the user is challenged with accessing a file with the name `--spaces in the filename--`

## My Approach

### First Attempt
I saw a sneak peak on how to solve this when I was researching how to access a file named `-` in the previous level. 

I discovered that if there are spaces in the filename, you can simply encase the filename with double quotes(`""`) or single quotes(`''`)

So I tried and inputted the command: `cat "--spaces in the filename--"`

### Result
```
cat "--spaces in this filename--"
error: unexpected argument '--spaces in this filename--' found

  tip: to pass '--spaces in this filename--' as a value, use '-- --spaces in this filename--'

Usage: cat [OPTION]... [FILE]...

For more information, try '--help'.
```

### Getting to the solution:
I got confused and went to check what could've went wrong. After going through google and some blog posts, the reason for this error is because the filename is prefixed with hyphens, which confuses the terminal into thinking that your entering a flag for an argument.

Hence, the error prompt of `unexpected argument`. To fix this, I have to input the flag `--` because it tells the terminal that anything typed after this flag will be counted as an argument. So, I tried again.

### The Solution
```
cat -- "--spaces in this filename--"
7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME
```
There's the password to the next level!

### Key Takeaway
Cheers to another way to access a file with an unusual name! Jokes aside, this level was a simple way to remind me that to research and find ways around these types of challenges.

### Tools/Commands Referenced
`ls` - list the contents of the current directory
`cat` - displays the file content.

<-- [Previous Write-Up](bandit-01-02.md) | [Next Write-Up](bandit-03-04.md) -->
