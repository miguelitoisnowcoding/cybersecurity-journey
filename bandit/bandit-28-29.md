# OverTheWire: Bandit Level 28 - 29
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

## The Challenge
Similar to the last level, you have to make a clone of the given repository (ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo) and see it's contents to access the details for the next level.

## My Approach
Just like the last level, I went cloned the given repository using `git clone` and checked the contents of it's repository.

Upon checking, I noticed that there was a `README.md` file that had this as it's contents.

```
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

This made me question myself. Am I supposed to decrypt this? It's only a markdown file, so I went and researched if there were any git commands that I didn't discover yet and there was.

Namely, `git log`, which was a command that checks the commit history of the repository. People can use the flag `-p` to see the changes done on a specified file. 
The format of the command would be `git log -p -- filename.type`, where `--` is a separator to indicate the break between the command/flags and the actual file.

### First Attempt
With that command in mind, we can run this command. `git log -p -- README.md`, which displayed these results.

### Result
```
commit 83d77407b76c9f86ac4e691a47618641c9d527ba (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jun 24 14:59:06 2026 +0000

    fix info leak

diff --git a/README.md b/README.md
index 42331d9..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: Em7eGtqaMySwNFjCpwzzHhLhospOcdt0
+- password: xxxxxxxxxx


commit 13bbc4d2414ffe0439b8ee4f5e5c2949780cf4b3
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jun 24 14:59:06 2026 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..42331d9 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
```

### Getting to the solution:
```
commit 83d77407b76c9f86ac4e691a47618641c9d527ba (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jun 24 14:59:06 2026 +0000

    fix info leak

diff --git a/README.md b/README.md
index 42331d9..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: Em7eGtqaMySwNFjCpwzzHhLhospOcdt0
+- password: xxxxxxxxxx

```

I can see in this commit that the password of `bandit29` was removed, indicated by the `--` and changed into the array of x's which was indicated by the `+-`.

### The Solution
So, the password for the next level is `Em7eGtqaMySwNFjCpwzzHhLhospOcdt0` and I found it using `git log -p -- README.md`

### Key Takeaway
In this level, I discovered that you can view the changes of a repository in git by using `git log`, which I find very useful especially when I need to check if any information was removed or added by another user.

<-- [Previous Write-Up](bandit-27-28.md) | [Next Write-Up](bandit-29-30.md) -->
