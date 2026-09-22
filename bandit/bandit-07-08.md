# OverTheWire: Bandit Level 07 - 08
Category: Linux Fundamentals | Date of Publish: 09/22/2026 | Difficulty: Easy

## The Challenge
In this level, the user is given a file named `data.txt`, it is a long list that contains words that are associated with a password. The user is tasked to locate the word *millionth* as the password is associated with it.

## My Approach

### First Attempt
One mistake that I made is using `cat` to manually check the file, but it resulted to me having to wait for a few seconds just for the file to display all it's contents.

### Result

```
...
shrubbery       OIpWNBxKiFZfJqJ0xiP5qZltYR7W9n2K
wiggler 2ojrgokcZKKbcVqb0TCKlMcPw5iF1QGK
spinner bTIgCiKlkeQeCnIi3APzLIMsY4ywPlNq
quenched        qfKqDso9XlMqHfAQYYZ8YsiPOncNQYsw
coppice RAwSXbq2EJQBKSA7bLpXspvhWVF5LP56
rendezvous's    7CfDoHeG5NemXiwFgLCVfeeEpCtPsLkF
tarnishes       ykRYFBeyvrsfwEVuhs2lzyOYFpClTtdT
skiers  jMYePWU5OAptjKxPOUPZUEqKaTaT8qIk
conceives       4byfXX8TSjOImVaceDBJConKEQWG21gH
metamorphosis's bInTU8mylbumlKh1TLxnZy0qKZqiN2O6
adorably        YmS7IXRqfv83oB8C7E0YKbTuUOn7dkbz
subcontractors  UUYtGD2oZsHp1LoOm6NlMjUUTfosHwgs
firebreak's     IKT9OBQZA91xoyseDlmMVnzNfZsWQehL
embargoed       1304KcKQgfoKJnSiGj88rLs0BWOCeR2P
revenue U3iUuEAgIo87cgGSMokP5RLZVIG0vKy9
impeded h4vgPYucM6xLKXTQb401wX89yCUkFSU0
notation's      IMClkvx6bsj9X2PSzFCtRxkntdr4RZbc
Erma's  InIFuOYrkM84rS5FYZVlXEDYxQVafjdb
Kiwanis's       NewyTBOdshdFcelXoBfd2oYbR4OSXD1R
oxymoron        r6yBxDh6uiizMiYL7OBRfO8ELXR1LnIB
mounts  fDia8hdSwlWmtbxK8u1cWeUTU9J14RP3
mouthpiece's    MCIMqSNelM9oGcV2nIZ8XnLF7fWC9kSc
Euphrates's     ikvJSuMBIUSGx4cK6iL1NhYFpGJeQqOH
dissed  ZqmpvsvAODN1EIj4oFAsNzPYJyc8OGdb
redeems Vf1AK3OeSjcQ9OE6hUfLCzQ6yvaggP1h
```

### Getting to the solution:
In this situation, there's actually a command that can immediately help us find the word `millionth` without having to scan through the entire text file and that command is `grep`.

`grep` is a command the finds text patterns in files, which is exactly what I need in this situation.

### The Solution
```
bandit7@bandit:~$ grep "millionth" data.txt
millionth       VR1ljMayciFxbnUokuQmJFw6QC9VKtub
```

### Key Takeaway
I was simply amazed on the use of grep and how it can easily scan through the contents of a file and find a pattern in the file.

### Tools/Commands Referenced
`grep` - finds patterns inside a file's contents.

<-- [Previous Write-Up](bandit-06-07.md) | [Next Write-Up](bandit-08-09.md) -->
