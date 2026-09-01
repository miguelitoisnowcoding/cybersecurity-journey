# OverTheWire: Bandit Level 23 - 24
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Hard

## The Challenge

Similar to the past two levels, the user is prompted to study the active cronjob of the current level and it's configurations. This level was noted to be a major jump from the previous levels as you need to make your own script to find out what the password for the next level is. 

## My Approach

### First attempt 
First, I went to the `/etc/cron.d` directory to see the files present using `ls`. After doing so, I noticed `cronjob_bandit24`, which was the last of the three cronjobs that users are able to access and view.

Second, I checked it's file type by using `file`, which came out as an ASCII Text File, then checked it out using `ls -l`; This is to check it's permissions. Then, I checked it's cron settings by using `cat`, which display the following: 

```
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

From this, we can say that it executes the cronjob at reboot because of the `@reboot` and that the owner of the cronjob is you, `bandit23`. We can also deduce that there is an *executable shell script* from the `/usr/bin/cronjob_bandit24.sh` and that the cornjob executes periodically every minute based on the `* * * * *`. 

What I did next was checking the shell script by using `cat /usr/bin/cronjob_bandit24.sh`, which displayed this:
```
#!/bin/bash

shopt -s nullglob

myname=$(whoami)

cd /var/spool/"$myname"/foo || exit
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done
```
From this code, I understood a few things. First, that it uses the current username as a variable, based on `myname=$(whoami)`. 

This line of code confused me for a second, `cd /var/spool/"$myname"/foo || exit`. As I thought it was supposed to be the start of a if-else statement or a ternary operator sequence. So, I checked if I am able to go to the `/var/spool/"$myname"/foo` by doing the command `cd /var/spool/"$myname"/foo`, but the system stated that it doesn't exist.

```
bandit23@bandit:/etc/cron.d$ cd /var/spool/bandit23/foo
-bash: cd: /var/spool/bandit23/foo: No such file or directory
```

So, when I ran the program, by doing `cronjob_bandit24.sh`. It resulted to: 
```
/usr/bin/cronjob_bandit24.sh: line 7: cd: /var/spool/bandit23/foo: No such file or directory
```

With that, I checked if I can use the commands from `cronjob_bandit23.sh`, but I wasn't able to access the password folder for `bandit24` because I lacked the permissions.

So, after doing basic checks, I started my making a temporary directory by using `mktemp -d`. There, I attempted to make a shell script by doing `nano first.sh`, but I was honestly confused on what to type down.

As a start, I copied the code of `cronjob_bandit23.sh`, but changed the `myname` variable to `bandit24`; as I thought this would make any difference.

```
#!/bin/bash

myname=bandit24
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

### Result

There I ran the code by doing `./first.sh` and it resulted to: 

```
Copying passwordfile /etc/bandit_pass/bandit24 to /tmp/ee4ee1703b083edac9f8183e4ae70293
cat: /etc/bandit_pass/bandit24: Permission denied
```

### Ruling things out

Overall, I believe I cannot access much of the files and information that we require from our current permissions as `bandit23`, which was evident that we can't properly use `cronjob_bandit23.sh` and `cronjob_bandit24.sh`

Second, we cannot just copy and paste the code from `cronjob_bandit23.sh` to our own script, thinking that it may do something different.


### The actual insight:

As I reviewed the code and from what I just said, I realized that because these shell scripts use `whoami` as the basis of their scope in the system, which some files or directories are inaccessible to me. 

Since, I'm under the username `bandit23`, directories like `/var/spool/"$myname"/foo ` are inaccessible to me. Therefore, we have to find a way to *utilize the permissions* of other users to access the permissions that we need to finish the level, which is the lesson of the past few levels.

Looking back, the level description never really explained where the password is located, so it made me wonder where I can find the password for `bandit24`. So, I researched and looked back at the levels I passed previously. There I remembered the directory of `/etc/bandit_pass` which contained all of the passwords of all bandit levels, but they can only be accessed by their respective username or by a username higher.


### Getting to the solution:

Since I can't access the `/var/spool/"$myname"/foo ` directory as `bandit23`, I can simply change the `"$myname" variable into `bandit24` to access the directory.

The command I used: `bandit23@bandit: cd /var/spool/bandit24/foo`

It worked! So, I checked there were any files inside the directory using `ls -la`, but it denied me from using it. This interaction made me want to check if I can add files into the folder, so I typed in the command `echo "test" > /var/spool/bandit24/foo/testfile.txt` then I `cat` it to check.
```
bandit23@bandit:~$ echo "test" > /var/spool/bandit24/foo/testfile.txt
bandit23@bandit:~$ cat /var/spool/bandit24/foo/testfile.txt
test
```

With that, I can deduce that you cannot view the contents of the `foo` directory, but you can add files into. Seeing that it has the permissions of `bandit24` and is executed every minute by `cronjob_bandit24` means that we have all the resources needed to access `etc/bandit_pass`.

### The Solution

In the temporary directory that we created beforehand, I edited the `first.sh` script into this:
```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/bandit24_password
```
breakdown: 
`#!/bin/bash` - After researching, this line of code is actually really important, as it tells the script to run using the Bash Interpreter.
`cat /etc/bandit_pass/bandit24 > /tmp/bandit24_password` - This line of code is basically saying "reduirect the value of /etc/bandit_pass/bandit24 into a bandit24_password, which is located in /tmp"
> note: the reason why we placed it in /tmp is because we can access that directory as `bandit23`

Before I make a copy of the script in `foo`, I made sure that the file is executable using `chmod` and the command looked like this `chmod +x first.sh`

Then I copied the `first.sh` script to `/var/spool/bandit24/foo` using `cp firsh.sh /var/spool/bandit24/foo`. With that, I waited for a minute, since `cronjob_bandit24.sh` is set to execute every minute.

After a minute, I viewed the `/tmp/bandit24_password` using `cat`

```
bandit23@bandit:/var/spool/bandit24/foo$ cat /tmp/bandit24_password
hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv
```

### Key Takeaway

"Can't access it? Find someone who can!" is how I would describe this level and the previous levels. Bandit Level 23-24 was another challenge that taught me to find ways to utilize the permissions of other users to access files and directories that can help you beat the level. From simply changing `$myname` to `bandit24` to access a directory, to making my own script to gain access to `/etc/bandit_pass` and transferring it's contents to a directory that I can personally access. 

### Tools/Commands Referenced
`cd` - To change directory
`ls` - List the contents of the current directory
`cat` - Print the contents of the file
`nano` - Allows you to edit the contents of a writable file
`cp` - Make a copy of a targetted file/directory
`mktemp -d` - Make a temporary directory
`file` - To check file type


[Optional: link back to a running index / previous write-up / next write-up]
