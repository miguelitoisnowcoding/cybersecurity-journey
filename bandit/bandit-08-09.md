# OverTheWire: Bandit Level 08 - 09  
Category: Linux Fundamentals | Date of Publish: 10/04/2026 | Difficulty: Easy

## The Challenge
The password for this level is stored in a file called `data.txt` and is the only line that appears once, the user has to go through the file and find the line that appears a single time.

## My Approach

### First Attempt
Upon seeing the challenge prompt, I immediately thought that the `uniq` command would be useful in this situation, since we're finding something that only occurs once. Before I used it, I used `man uniq` to check if it has any useful flags that I can use to narrow down the search, and there I found two.

Specifically, `-u` which will bring up unique lines and `-c` to indicate how many times they've appeared, but upon using it displayed a series of passwords which confused me a bit.

### Result
```
bandit8@bandit:~$ uniq -c -u data.txt
      1 93zS64a8V2Lb9UEh2Jflq30EJwZ8qkZ8
      1 BaqmjXxHkTrJn7xgeSgR8yDtDKTHw2aK
      1 HIBYBHDXBVTV7POllMsRYz4VBxp1I5Bu
      1 aOeMCMb4NigpOYNWqJ7HjBwNs9CnKicJ
      1 xLqeaWqsfixvQRirU3ZVgdsCeDb6bezo
      1 9mTgDtYNkscji4iicxAmKJ6T88SxB3AU
      1 3VQqeGb3jcNmy510RliBj4WmBG0dXq8b
      1 W1D6EBetQeDxZzmbdX0xsZCWMj8HVhIv
      1 1uO2FE90IVExrUXcUeTxrNeGny2OoXLL
      1 K4roE2HAD3BqcEXVXz5n0aorOKptGW9U
      1 tIlMXEZLDanAWcvYwEl8d0vGlOV26dsz
      1 0DbSorboUQPx0T3Vo4uKuOY1KkgkBno6
      1 35YXS7pKRO8UjKsKImDct5mOAYKtek7j
      1 EiXS1PPA6KbMMmj2yCWsemWxd5cH4l90
      1 bakPcXvEd3lQheFQMtSL7u9O1bypRZl0
      1 KEktczgPBlbD9DOS9Prfmka3fNj3FPfL
      1 m48CiahsjJxHYwqVZvBeIxU35YxhfFnE
      1 hAkFBDkXukQpOolLGF3ZvqwgSZVO4Lut
      1 9mTgDtYNkscji4iicxAmKJ6T88SxB3AU
      1 1k1ZAkCuOnsH3c4LuzeEesExQHFq6CS0
      1 3VQqeGb3jcNmy510RliBj4WmBG0dXq8b
      1 HVBC2Zd31CpsblpDOn9SlOQcEXBhmgcR
      1 My9OjG4U5zZAzHqynEKwcliZqirvTcRD
      1 nHUmskd8miYt7ihd66tVhLNU1h5JbPk7
      1 ZGsUseu5PhfXdMvx5TetTKNMEs0plCIT
      1 35YXS7pKRO8UjKsKImDct5mOAYKtek7j
      1 aOeMCMb4NigpOYNWqJ7HjBwNs9CnKicJ
      1 hW9L5mF2NVjegnl1xHJxdRhG5kPiNd2j
      1 FrfnJvq7PR19g0nXj0vICuDu5yVY02ET
      1 wmqvH7X8wDz4pFqwp4E87LKYqOSyqKrN
      1 bagqG4H8asx57KUkrwwwVD1piLqbsarl
      1 HFLjLTY2nU0FviL3zOI1Xsx6v2lcO6TT
```

### Getting to the solution:
There I thought of combining the `sort` command in the previous `uniq` command line we used, as I thought sorting the file before going through it would help the `uniq` command find and rule out the password easier. 

So I created a pipeline with `sort` and `uniq`, so the command looked like this: `sort data.txt | uniq -u -c`

What it would do is that it'll sort the contents of data.txt and since `uniq` checks the lines adjacent to the current line it's on, it'll have an easier time going through and filtering out the duplicate lines in the file.

### The Solution
```
bandit8@bandit:~$ sort data.txt | uniq -u -c
      1 EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl
```

### Key Takeaway
Again, still amazed with how useful some commands are and how strong they are when paired with other commands. This level also reminded me that it's better to sort a file first before going through it.

### Tools/Commands Referenced
`sort` - sorts the file
`uniq` - finds and flags out duplicate lines that its adjacent to.

<-- [Previous Write-Up](bandit-07-08.md) | [Next Write-Up](bandit-09-10.md) -->
