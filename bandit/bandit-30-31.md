# OverTheWire: Bandit Level 30 - 31
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

## The Challenge
The challenge continues to revolve around utilizing the tools and commands provided by git. In this level, you are provided another git link with is `ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo` via the port 2220. The password for `bandit30-git` is the same with the current level's password.

## My Approach

### First Attempt
As usual, I cloned the git repository and checked the contents of the directory. I noticed that there was the `README.md` file and opened it.

```
migz@Miguel:~/repo$ cat README.md
just an epmty file... muahaha
```

Here, I was confused, so I checked the logs of the file from across the branches using `git log --all -p -- README.md`

```
commit 8f2cf5b700aa0dc83b8ec69f46974e81dd023e99 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jun 24 14:59:10 2026 +0000

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..029ba42
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+just an epmty file... muahaha
```

### Getting to the solution:
Seeing this made me even more confused because it means there's only been one commit across all the branches of this repository.

From here, I thought that the typo of *epmty* was a hint in solving this level. So, I searched if there were any commands that would reveal any hidden files. 

There I discovered a command called `git tag` which is supposed to reveal any commit or details that was hidden branches and commits. Apparently, *tag* is a separate feature of git that is similar to commit and branch, where you can hide certain details or commits.

```
migz@Miguel:~/repo$ git tag
secret
```

To show these *tags*, you can use `git show [tag name]` to showcase the contents of the tag.

```
migz@Miguel:~/repo$ git show secret
82NkymblpGBYmIXG6ZQ8YldBYstHpfUf
```

### The Solution
Password for `bandit32` is `82NkymblpGBYmIXG6ZQ8YldBYstHpfUf`

### Key Takeaway
Discovering the git commands `tag` and `show` was interesting to me, as I never knew they existed. This made me realize that certain information and details can be hidden from users or even other developers in these git repositories, which gives me this curiosity of how `tag` is used in the day-to-day life of a developer and how developers can use it to store sensitive information like passwords.

### Tools/Commands Referenced
`git tag` — showcase the tags hidden in the repository
`git show [tag name`] — showcase the contents of the tags


<-- [Previous Write-Up](bandit-29-30.md) | [Next Write-Up](bandit-31-32.md) -->
