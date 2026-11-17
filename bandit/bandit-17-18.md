# OverTheWire: Bandit Level 17 - 18
Category: Linux Fundamentals | Date of Publish: 11/17/2026 | Difficulty: Easy

## The Challenge
The level presents the user with two files, namely `passwords.new` and `passwords.old`, the password of the next level is stored in `passwords.new` but is the only lined that was changed from `passwords.old`. The goal is to find that change.

## My Approach

### Getting to the solution:
When I looked at the list of suggested commands for the current level, I noticed the command called `diff` and after reading it's manual, I discover it can see the difference between two files.

```
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< 1WJHmb0k7TH4piWp9Pd0lsruErLQZrR9
---
> OQxXZjELndr90zuhOTDYBEomI0SZITXI
```

### The Solution
The password is `OQxXZjELndr90zuhOTDYBEomI0SZITXI`.

<-- [Previous Write-Up](bandit-16-17.md) | [Next Write-Up](bandit-18-19.md) -->
