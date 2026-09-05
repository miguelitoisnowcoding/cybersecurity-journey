# OverTheWire: Bandit Level 27 - 28
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

## The Challenge
We are introduced to Git! The challenge is to clone the `ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo` repository via port 2220. Note, that the password for `bandit27` is the same for the `bandit27-git` repository.

## My Approach
For context, I've had experience with git and github before because of my time doing Python, JavaScript, and in most of the hackathons I've attended, so I'm familiar with things commands such as `push`, `pull`, `commit`, and the command that we need in this challenge, which is `clone`.

### First Attempt
```
migz@Miguel:~$ git clone  ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

> The only thing different from usual git clones in my experience is the need to specify the port, which can be seen from the use of the `:` after the server address.

### Result
```
Cloning into 'repo'...
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit27-git@bandit.labs.overthewire.org's password:
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (3/3), done.
```

### Getting to the solution:
After using `ls` to check if I got any new files, I noticed that theres a new repository noted as `repo`.

There I went inside of the file using `cd` and saw that there was a `README` file in the directory. So, I used `cat` and there it was.

### The Solution
```
migz@Miguel:~/repo1$ cat README
The password to the next level is: y8Yd2ssKcpHpud7UvOSOxwamRMzIGIeQ
```

### Key Takeaway
This level was a bit more relaxing to me because of how I was already familiar with *git*'s commands and it was another reminder to me of how cool and useful git is, as a tool to a developer's life.


<-- [Previous Write-Up](bandit-26-27.md) | [Next Write-Up](bandit-28-29.md) -->
