# OverTheWire: Bandit Level 27 - 28
Category: Linux Fundamentals | Date: 09/1/2026 | Difficulty: Easy

## The Challenge
"Simple and Easy" is the level. You literally just go and get the password using the knowledge we gained in the previous level.

## My Approach
Logging back into `bandit26`, I noticed that there is a *setuid* file on the home directory, called `bandit27-do`.

In previous levels, I learned that *setuid* files temporarily give you access it's owner's permissions and access. With this in mind, I can access the `/etc/bandit\_pass/bandit27` without any difficulty.

First things first, let's run it by itself to see if there's any requirements to it.

### First Attempt
`bandit26@bandit:~$ ./bandit27-do`

### Result
```
bandit26@bandit:~$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id
```

### Getting to the solution:
There, I saw that its asking me to run a command as another user, so I would just need to display the contents of `/etc/bandit\_pass/bandit27` using `cat`.

### The Solution
```
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit\_pass/bandit27
STJLJBRRphMxKB392CT4iOr5CbzPU9ER
```

### Key Takeaway
This level was a simple reminder that not all levels are difficult hurdles that would require you to decompress a file or to create a script to find you a pin code. Sometimes, the challenges are direct and easy, such as this one; where you just need to know the purpose of the file and make use of it.

<-- [Previous Write-Up](bandit-25-26.md) | [Next Write-Up](bandit-28-29.md) -->
