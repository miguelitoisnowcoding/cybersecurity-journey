# OverTheWire: Bandit Level 29 - 30
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

## The Challenge
The same with the past two levels, but the repository now is `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` at port 2220

## My Approach
As usual, I cloned the repository and went inside to check the contents. There I found another `README.md` with the contents of:

```
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```

Seeing this, it made me want to check the logs of the repository.

### First Attempt

So, I used `git log -p -- README.md`

### Result
```
commit a9c5d1c2b43890809f3077bb9ec65c30ced242fb (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jun 24 14:59:08 2026 +0000

    fix username

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
--- a/README.md
+++ b/README.md
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit.

 ## credentials

-- username: bandit29
+- username: bandit30
 - password: <no passwords in production!>


commit b548c69e5c7db36002cf8382ad49b4b3f883da71
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jun 24 14:59:08 2026 +0000

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..2da2f39
--- /dev/null
```

### Getting to the solution:
I saw nothing, which made me wonder, where could it be? I thought about it and remembered that it's etiquette or rather it's recommended to have a separate branch for files in production, which is typically called development or dev.  

So, I checked it by using `git branch -a`. The `-a` flag is necessary because it displays all the active branches in the repository.

```
migz@Miguel:~/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

There it is, the dev branch, which should contain any code or commits that are in development that still require checking. I used `git checkout dev` to leave the current branch(master) to move into dev and after checking it's logs I saw this.

```
migz@Miguel:~/repo$ git checkout dev
branch 'dev' set up to track 'origin/dev'.
Switched to a new branch 'dev'
migz@Miguel:~/repo$ git log -p -- README.md
commit d36874ce7e88201c326bb596ba47a4cd063a023e (HEAD -> dev, origin/dev)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jun 24 14:59:08 2026 +0000

    add data needed for development

diff --git a/README.md b/README.md
index 1af21d3..d395d04 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for bandit30 of bandit.
 ## credentials

 - username: bandit30
-- password: <no passwords in production!>
+- password: jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX


commit a9c5d1c2b43890809f3077bb9ec65c30ced242fb (origin/master, origin/HEAD, master)
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jun 24 14:59:08 2026 +0000

    fix username

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
--- a/README.md
+++ b/README.md
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit.
```

### The Solution
These lines in log contain the password to the next level.
```
- username: bandit30
-- password: <no passwords in production!>
+- password: jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX
```

### Key Takeaway
What I learned from this level is to always check if there are any other places where information can be kept. Similar to how the password for `bandit30` wasn't kept or initially place in the `master` directory, but was inside `dev` as it was under production.

<-- [Previous Write-Up](bandit-28-29.md) | [Next Write-Up](bandit-30-31.md) -->
