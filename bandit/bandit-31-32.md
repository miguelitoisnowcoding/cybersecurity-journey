# OverTheWire: Bandit Level 31 - 32
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

## The Challenge
Another git challenge! The git provided was `ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo` at port 2220. In the repository, you are given a `README.md` file with the text:
```
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

Instead of being the one to receive or find the file, you're the one who's gonna send it.

## My Approach

### First Attempt
I started off by creating a file called `key.txt` using `nano` and inputted the text of *"May I come in"* in the file. 

After doing so, I did `git add key.txt` to prepare and give notice to git that I have a file to commit.

Then, I did `git commit -m "May I come in?"`, which adds all the files I listed to this commit.

Finally, I did `git push origin master` to send the files to the remote repository.

### Result
```
migz@Miguel:~/repo$ git push origin master
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit31-git@bandit.labs.overthewire.org's password:
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 326 bytes | 65.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
To ssh://bandit.labs.overthewire.org:2220/home/bandit31-git/repo
```

### The Solution
here's the password of `bandit32: pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT`

### Key Takeaway
Nothing much to bring say here as I'm really familiar with these tools.


<-- [Previous Write-Up](bandit-30-31.md) | [Next Write-Up](bandit-32-33.md) -->
