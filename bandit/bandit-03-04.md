# OverTheWire: Bandit Level 03 - 04
Category: Linux Fundamentals | Date of Publish: 09/15/2026 | Difficulty: Easy

## The Challenge
The challenge is to find file the contains the password, which is hidden in a directory called `inhere`. 

## My Approach

### First Attempt
First things first, I checked the home directory using `ls` and saw the directory `inhere` so I changed directories using `cd inhere`.

There I checked using `ls` and noticed that it was completely empty.

### Result
```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls
```

### Getting to the solution:
So, I used `-la` flag of `ls` to see the every content in the directory including hidden files and their details.

```
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Jun 24 14:59 .
drwxr-xr-x 3 root    root    4096 Jun 24 14:59 ..
-rw-r----- 1 bandit4 bandit3   33 Jun 24 14:59 ...Hiding-From-You
```

There I saw the file Hiding-From-You, so I copied it's location and used `cat` to display it's contents.

### The Solution
```
bandit3@bandit:~/inhere$ cat ...Hiding-From-You
xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq
```

### Key Takeaway
This was a nice opening to hidden files and accessing/finding them in security contexts. 

### Tools/Commands Referenced
`ls` - lists the contents of the directory.
`cd` - changes the directory.
`cat` - display file contents.
`ls -la` - displays file details of all files in the directory.

<-- [Previous Write-Up](bandit-02-03.md) | [Next Write-Up](bandit-04-05.md) -->
