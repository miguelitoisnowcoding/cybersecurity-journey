# OverTheWire: Bandit Level 11 - 12
Category: Linux Fundamentals | Date of Publish: 10/06/2026 | Difficulty: Intermediate

## The Challenge
In this level, the password is stored in a data.txt file where all the letters have been moved or rotated 13 steps, so the user is prompted to find a way to decode this base-13 encryption and decode the password.

## My Approach

### First Attempt
First things first, I went through the *Helpful Reading Material* since it's the first thing I noticed at the page. After reading it, I got a better idea on what the level meant that the letters were rotated 13 positions.

When I logged in the bandit level, I first checked the `data.txt` file using `cat`.

### Result
```
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf TEBbmJCB8DlA0zTewHxVQ0JPLxMvDkeA
```

### Getting to the solution:
There, I confirmed that it was really rotated 13 positions, so I looked through the suggested commands and checked what commands I haven't used yet. 

I saw that there was a translate command, which was noted as `tr`, and went through it's manual.

It works by passing in two character sets, `set 1` being the set of characters that you want to change and `set 2` being the set of characters you want to replace the previous set with. Since, the characters are positioned 13 steps forward, I need to set it so that the characters `a-z` and `A-Z` take into the account the change.

Therefore, I set the first set to be `a-zA-Z` to take care of the uppercase and lowercase letters, then I set the second set to take into account 13-step rotation for both uppercase and lowercase letters. So, I made the second set `n-za-mN-ZA-M`.

There, I pipelined the `tr` command with the specified sets with `cat`.

### The Solution
```
bandit11@bandit:~$ cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M'
The password is GROozWPO8QyN0mGrjUkID0WCYkZiQxrN
```

### Key Takeaway
Here, I learned to process the information carefully, since the level revolved around the concept of the base-13 or ROT13 encryption and I had to use that knowledge to create a command that will address the rotation of the letters.

### Tools/Commands Referenced
`tr` - translate a text by substituting characters from a specified set.

<-- [Previous Write-Up](bandit-10-11.md) | [Next Write-Up](bandit-12-13.md) -->
