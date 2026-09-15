# OverTheWire: Bandit Level 04 - 05
Category: Linux Fundamentals | Date of Publish: 09/15/2026 | Difficulty: Easy

## The Challenge
Another `inhere` directory challenge. In this level, the user is challenged to find the only human-readable file among a collection of files in the `inhere` directory.

## My Approach

### First Attempt
As per usual, I went straight to the `inhere` directory to check what files are present in the directory. Once in, I checked using `ls` and saw:

```
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```

There I thought of checking (`file`) and displaying (`cat`) the contents of each file, but I thought that it would take too long and is inefficient. So, I went through the manual of `file` to check if there's anyway to check all the files in a directory.

There I discovered the option of `*` which simply tells `file` to check the file type of any file that follows any sequence of characters. So, I tried it.

### Result

```
bandit4@bandit:~/inhere$ file *
file: Cannot open `ile00' (No such file or directory)
file: Cannot open `ile01' (No such file or directory)
file: Cannot open `ile02' (No such file or directory)
file: Cannot open `ile03' (No such file or directory)
file: Cannot open `ile04' (No such file or directory)
file: Cannot open `ile05' (No such file or directory)
file: Cannot open `ile06' (No such file or directory)
file: Cannot open `ile07' (No such file or directory)
file: Cannot open `ile08' (No such file or directory)
file: Cannot open `ile09' (No such file or directory)
```

### Getting to the solution:

There I assumed it's because there's a hyphen in the file name of each file, so I did it again, but this time I prefixed the `*` with `./` so that it includes files with hyphens in them.

```
bandit4@bandit:~/inhere$ file ./-*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: OpenPGP Public Key
./-file07: ASCII text
./-file08: data
./-file09: Motorola S-Record; binary data in text forma
```

There we can see that there is a human-readable file, which is `-file07`, who is listed as a ASCII text. So, I `cat` it to check it's contents.

### The Solution
```
bandit4@bandit:~/inhere$ cat ./-file07
6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
```

### Key Takeaway
Here's another level of me discovering new flags and options to use different commands to have more variety and to address specific issues.

### Tools/Commands Referenced
`ls` - lists the files of the directory.
`cat` - displays file contents.
`file` - displays the file type of the specified file.

<-- [Previous Write-Up](bandit-03-04.md) | [Next Write-Up](bandit-05-06.md) -->
