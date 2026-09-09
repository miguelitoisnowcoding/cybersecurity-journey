# OverTheWire: Bandit Level 00 - 01 
Category: Linux Fundamentals | Date of Publish: 09/9/2026 | Difficulty: Easy

## The Challenge
The challenge was to locate a file called `readme` in the home directory because it contains the password for the `bandit1`, which the user has to connect to using ssh.

Commands you may need to solve this level
`ls` , `cd` , `cat` , `file` , `du` , `find`

## My Approach

### First Attempt
I started of by using `ls`, which is used to list down the contents of the current directory, to check for the file `readme` in the directory. 

### Result
```
bandit0@bandit:~$ ls
readme
```

### Getting to the solution:
There I used `cat`, which is used to display the contents of a file.

### The Solution
```
bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walkthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: 6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR
```

### Key Takeaway

### Tools/Commands Referenced
`ls` - listed down the contents of the current directory.
`cat` — displays the contents of a specified file.

<-- [Previous Write-Up](bandit-0.md) | [Next Write-Up](bandit-01-02.md) -->
