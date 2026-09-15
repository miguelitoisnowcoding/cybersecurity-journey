# OverTheWire: Bandit Level 05 - 06
Category: Linux Fundamentals | Date of Publish: 09/15/2026 | Difficulty: 

## The Challenge
The challenge in this level is to find the file that contains the next level's password in a directory called `inhere`. However, it is hidden in one of the listed directories,
but one thing to note is that it's only *1033 bytes*, *not executable*, and is *readable*.

## My Approach

### First Attempt
Upon arriving in the current level, I went to the `inhere` directory and checked it's contents using `ls`

### Result
```
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18
maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19
maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17
```

### Getting to the solution:
Here, I thought that it would be inefficient and time consuming if I were to go through each directory and check all the files it contains. So, I utilized the specified details that bandit provided, which were: 

- 1033 Bytes
- not exectuable
- human readable

What I first did was to use `find` with the parameter and value of `-size 1033c` to check if there any files that are exactly 1033 bytes big.

```
bandit5@bandit:~/inhere$ find -size 1033c
./maybehere07/.file2
```

### The Solution
I found it's location, so I `cat` it to see it's contents.
```
cat ./maybehere07/.file2
pXa26xhMWaC2SvDotA4r9EgZkulOeSBW
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        bandit5@bandit:~/inhere$
```

### Key Takeaway
This was another challenge that showcased the importance of efficiency and the maximization of commands that *bandit* has shown us.

### Tools/Commands Referenced
`ls` - lists down the files in a directory.
`find` - finds files depending on the given details.
`cat` - displays the contents of a file. 

<-- [Previous Write-Up](bandit-04-05.md) | [Next Write-Up](bandit-06-07.md) -->
