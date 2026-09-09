# OverTheWire: Bandit Level 01 - 02
Category: Linux Fundamentals | Date of Publish: 09/9/2026 | Difficulty: Easy

## The Challenge
In this next level, the user has to access a file named `-` which is located in the home directory.

## My Approach

### First Attempt
The first thing I did was checking the directory using `ls` then I noticed the `-` file, so I used `cat` to check the contents of the file, but it didn't display anything

### Result
```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat -

```

### Getting to the solution:
So, I instantly thought that it was because of how the file is named `dash` which could be interpretted as something different.

Therefore, I did some research and discovered that you can do a variety of prefixes to display filenames with special characters, but in this situation you can use `./` as it's prefix.

### The Solution
So, I tried and found the password for the next level.
```
bandit1@bandit:~$ cat ./-
PK8fYLZg2hnHSz83plBL1iEPKdD3QToB
```

### Key Takeaway
This level reminded me of how you need to utilize a similar prefix in other programming languages to display certain characters like `"` and `'`

### Tools/Commands Referenced
`cat` — display file's contents
`ls` - list directory's contents

<-- [Previous Write-Up](bandit-00-01.md) | [Next Write-Up](bandit-02-03.md) -->
