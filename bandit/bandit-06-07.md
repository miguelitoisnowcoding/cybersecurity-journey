# OverTheWire: Bandit Level 06 - 07
Category: Linux Fundamentals | Date of Publish: 09/22/2026 | Difficulty: Easy

## The Challenge
The obstacle that the user has to face in this current situation is to find a file *hidden in the server*. This challenge is different because compared to previous levels, the file isn't found in the home directory. However, the challenge provides the file's specifications, which are:

```
owned by user bandit7
owned by group bandit6
33 bytes in size
```

## My Approach

### First Attempt
In my first, attempt I tried checking the directory using `-ls -la` but returned with no useful information.

```
total 20
drwxr-xr-x   2 root root 4096 Jun 24 14:58 .
drwxr-xr-x 150 root root 4096 Jun 24 15:02 ..
-rw-r--r--   1 root root  220 Feb 13  2026 .bash_logout
-rw-r--r--   1 root root 3851 Jun 24 14:50 .bashrc
-rw-r--r--   1 root root  807 Feb 13  2026 .profile
```

Then, I noticed that there was another directory I could access, which was `..` and I checked there.

```
bandit6@bandit:/home$ ls -la
total 600
drwxr-xr-x 150 root         root         4096 Jun 24 15:02 .
drwxr-xr-x  28 root         root         4096 Aug 25 21:07 ..
drwxr-xr-x   2 root         root         4096 Jun 24 14:58 bandit0
drwxr-xr-x   2 root         root         4096 Jun 24 14:59 bandit1
drwxr-xr-x   2 root         root         4096 Jun 24 14:58 bandit10
drwxr-xr-x   2 root         root         4096 Jun 24 14:58 bandit11
drwxr-xr-x   2 root         root         4096 Jun 24 14:58 bandit12
drwxr-xr-x   2 root         root         4096 Jun 24 14:59 bandit13
drwxr-xr-x   3 root         root         4096 Jun 24 14:59 bandit14
drwxr-xr-x   2 root         root         4096 Jun 24 14:59 bandit15
drwxr-xr-x   2 root         root         4096 Jun 24 14:59 bandit16
drwxr-xr-x   3 root         root         4096 Jun 24 14:59 bandit17
drwxr-xr-x   2 root         root         4096 Jun 24 14:59 bandit18
drwxr-xr-x   2 root         root         4096 Jun 24 14:59 bandit19
drwxr-xr-x   2 root         root         4096 Jun 24 14:59 bandit2
...
```

So, I moved to using `find` using the given file details as flags to narrow down the scope of the command.

### Result
```
bandit6@bandit:/home$ find -group bandit6 -user bandit7
find: ‘./ubuntu’: Permission denied
find: ‘./bandit5/inhere’: Permission denied
find: ‘./drifter8/chroot’: Permission denied
find: ‘./bandit28-git’: Permission denied
find: ‘./drifter6/data’: Permission denied
find: ‘./bandit29-git’: Permission denied
find: ‘./bandit30-git’: Permission denied
find: ‘./leviathan0/.backup’: Permission denied
find: ‘./bandit31-git’: Permission denied
find: ‘./leviathan4/.trash’: Permission denied
find: ‘./bandit27-git’: Permission denied
```

### Getting to the solution:
I went back to the original directory and tried something different. I added two more flags, which are the `/`, `-type f`, and `2>/dev/null`.

`/` is a flag that tells the `find` command to check through the root of the filesystem and to all of it's branches. `-type f` on the other hand, simply tells `find` that were finding a file. Lastly, `2>/dev/null` puts all the unnecessary output like the `permission denied` messages to a void, so that the only that will out are the one's I actually need.

The command will then turn out like this: `find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null`

### The Solution
```
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3
```

### Key Takeaway
I was simply amazed on how powerful and useful commands like `find` are in searching and scanning through a file system and how convenient flags like `/` and `2>/dev/null` are.

### Tools/Commands Referenced
`find` - to find specific files

<-- [Previous Write-Up](bandit-05-06.md) | [Next Write-Up](bandit-07-08.md) -->
