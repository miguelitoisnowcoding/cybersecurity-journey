# OverTheWire: Bandit Level 13 - 14
Category: Linux Fundamentals | Date of Publish: 10/07/2026 | Difficulty: Easy

## The Challenge
This level is unique because you don't need to find a password for the next level, but rather, there is a file called a sshkey called `sshkey.private` in the home directory of bandit. The user has to use this key and the commands they've used in previous levels to move to the next level.

## My Approach

### First Attempt
When I arrived in the level, I checked the directory for it's files and saw two files, `HINT` and `sshkey.private`. I checked out the `HINT` file to see what hint it was for me.

```
bandit13@bandit:~$ ls
HINT  sshkey.private
bandit13@bandit:~$ cat HINT
If you have trouble with this level, note the following:

1) As for all other levels, this level has a website with information:
   https://overthewire.org/wargames/bandit/bandit14.html
2) No, the level is not broken. To verify, see:
   https://status.overthewire.org/
3) The current version of OverTheWire prevents logging in from one
   level to another via localhost. Log out, and see 1)
4) If you get errors, read the error message on your screen.
   We mean it!
```

With this, I went through the list of suggested command that the level recommends to use and the command of `scp` caught my eye. `scp` or secure copy, copies files from a specified or remote host and places the copy to your desired destination.

So, I disconnected from the bandit level and tried it for myself.

### Result
```
migz@Miguel:~$ scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private ~
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit13@bandit.labs.overthewire.org's password:
sshkey.private                                                                                                                          100% 2602     4.1KB/s   00:00
```

### Getting to the solution:
After doing that, I checked my directory using `ls` to check if the file was actually there and it was. I assumed that since it was called sshkey, I had to use `ssh` to utilize the key for the next level.

I went through it's manual and discovered the `-i` flag, which allows me to pass in a sshkey or a file that will be passed as the password for the next level.

### The Solution
```
migz@Miguel:~$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

backend: gibson-0

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!
```

### Key Takeaway
This was an interesting level because it showed me that you won't always need a password to access another level or to gain access to a server.

### Tools/Commands Referenced
`scp` - safely copies a file from a source to a designated place.
`ssh` - connects yourself to a remote server.

<-- [Previous Write-Up](bandit-12-13.md) | [Next Write-Up](bandit-14-15.md) -->
