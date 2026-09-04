# OverTheWire: Bandit Level 25 - 26
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Intermediate

## The Challenge
"Something is stopping me from connecting to the next level, but what's stopping me?" Was the exact thought that passed through my head when I faced this level. To put it simply, `bandit26` doesn't have the usual configuration of `!/bin/bash` which in past levels was telling the program to run using the Bash Interpreter Binary. Hence, you have to find a way to *break out of it.* 

We are also introduced to a new set of commands, namely `more`, `vi` and `id`.

## My Approach
The moment I logged in `bandit25`, I checked if there were any files in the home directory using `ls` and there was a file called `bandit26.sshkey`. 

Instinctively, I logged out of bandit25 and used `scp` to have upload the sshkey into my local device because I found it easier to work that way. After downloading it I immediately checked if you could connect to `bandit26` instantly by doing `ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220`, but I was disconnected immediately. 

```
Enjoy your stay!

  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
Connection to bandit.labs.overthewire.org closed.
```

At first I thought it was just and error, but after trying another time, I finally noticed the extra **bandit26** ASCII art at the end. So, I started checking out the commands that this level introduced and the `more` command piqued my interest. 

The purpose of the `more` command was to display large documents neatly and immediately close after being used. This command was an alternative to `cat` because it typically displays texts or details instantly with no way of going through it cleanly, which made going through big documents difficult.

After researching, I discovered that `more` has a secondary feature that most Unix Interactive tools like vi and less also share, which is a feature called *shell escape*. Basically, it allows the user to pause processes of more and enter any shell command, this knowledge will come useful later on.

I connected back into `bandit25` to check a file called `passwd` located at `/etc/passwd` which is stated to contain details of all the files in the bandit system and upon checking the details of `bandit26`, it displayed this:
```
bandit25:x:11025:11025:bandit level 25:/home/bandit25:/bin/bash
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```
Which confirms that `bandit26 isn't running on the usual `!/bin/bash`, so I checked the file `showtext` and it displayed this: 

```
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

The line `exec more ~/text.txt` is the culprit in this script. To recap, `more`'s primary use is to act as a pager for large documents or text files but because the `text.txt` file is so short, it closes the file instantly. Hence, why I was immediately disconnected in my last attempt in connecting to `bandit26`.

### First Attempt

Because the text file is so small, I had to find a way to delay `more` from displaying all the contents of the `text.txt`. After looking into `more` and some research, I can delay it by making my terminal smaller, thus delaying `more` from showing the entire file because there's less space to showcase text.

So, I tried connecting to `bandit26` again, but with a smaller terminal and it worked!

### Result

```
  _                     _
_ _   ___   __
 | |                   | (
_) | |__ \ / /
 | |__   __ _ _ __   __| |
--More--(43%)
```

One of the other things I've learned about this interactive window of `more` is that you can open it as an editor by pressing `v`, but this is where I got stuck because I wasn't quite sure what to do.

### Ruling things out

As stated in the challenge description and by the details provided by `/etc/passwd`, `bandit26` doesn't follow the usual `/bin/bash` configuration, so we have to find a way to change that.

### The actual insight:
Doing some more research, I saw that you can change the Shell Setting by using `set` followed by the `shell=" and then the shell type of your choosing, which in our situation is `/bin/bash`. So, the complete command would be `:set shell=/bin/bash`, after that command I can initiate it by typing down `:shell` which allowed me to enter `bandit26`.

### Getting to the solution:
Now the rest is simple, since I had the permissions of `bandit26`, I went straight to `/etc/bandit\_pass/bandit26`.

> recap: '/etc' contains all of bandit data and is readable by everyone. However, information such as the passwords of bandit levels are only accessible by their respective bandit user or a bandit user of a higher authority.

### The Solution
There I used `cat` to display the contents of that location and there it was
`jHdv2ELQhT22BkprMNDjybZDAkw1zeBJ`

### Key Takeaway
This level was another "think outside the box" moment for me, it taught me that to really read into the features of a command and see if there's a way to utilize it in to my advantage. In this situation where there was a restricted environment because of the `more` command's function of displaying the text.txt file, I had to find a way in delaying it's processes, which was to make my terminal window smaller so it had less space to display the text.

[Optional: link back to a running index / previous write-up / next write-up]
