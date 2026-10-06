# OverTheWire: Bandit Level 12 - 13
Category: Linux Fundamentals | Date of Publish: 10/06/2026 | Difficulty: Intermediate

## The Challenge
This level is similar to the previous ones, where you have to decode a file called data.txt to find the password for the next level. However, the user is now prompt to make a place to deal with the txt file because the data.txt has been repeatedly compressed and the user has to decompress it to uncover the next password.

## My Approach

### First Attempt
The first thing I did was what the level recommended to do first, which is to create a temporary directory where I can place a copy of the file, so I can safely work on it.

There, I made a copy of the `data.txt` file using `cp` and moved it into the temporary directory, which I made by doing `mktemp -d`. Once, I was in the temporary directory, I checked the file out using `file` and `cat`

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ file data.txt
data.txt: ASCII text
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ cat data.txt
00000000: 1f8b 0808 0d3f b86a 0203 6461 7461 322e  .....?.j..data2.
00000010: 6269 6e00 0148 02b7 fd42 5a68 3931 4159  bin..H...BZh91AY
00000020: 2653 594b 1a6d 4300 0018 ffff ffcb 57cf  &SYK.mC.......W.
00000030: f2f7 97f7 cdff fff7 bfff e7cf 8dfe afff  ................
00000040: 3fbf ffef f4df 657f 3afb e7ff b001 3b30  ?.....e.:.....;0
00000050: 1068 3234 0068 0643 4064 00d3 2001 a000  .h24.h.C@d.. ...
00000060: 0000 6806 8680 0346 8680 01a0 0001 a326  ..h....F.......&
00000070: 9903 087a 8d18 d27a 8da4 1c8f 48d0 3468  ...z...z....H.4h
00000080: 00d0 034d 034d 0006 2340 d0d0 d000 c8d0  ...M.M..#@......
00000090: 0068 0f51 a623 4000 01a6 8680 64c9 a680  .h.Q.#@.....d...
000000a0: d184 0006 9900 e9ea 0320 1903 400d 1a68  ......... ..@..h
000000b0: 0190 0000 d064 681a 000c 4c86 8068 1a64  .....dh...L..h.d
000000c0: 01a6 868d 1a68 6231 340d 0d0c 40d0 1a1a  .....hb14...@...
000000d0: 0000 000a 5000 5c95 950f 5520 e655 18e9  ....P.\...U .U..
000000e0: 34e5 937e e6c3 d58e 81e1 f4c6 5664 5e33  4..~........Vd^3
000000f0: 3c00 cdbc 1797 54f6 f703 61c0 3e65 9138  <.....T...a.>e.8
00000100: 43a3 c6c4 9c71 1f71 4995 0839 e2fe 9ef7  C....q.qI..9....
00000110: 0f69 86a6 6ec4 5920 270f 5bae 953a 37c8  .i..n.Y '.[..:7.
00000120: c950 3300 6dc6 3ed6 9ce1 04d9 2216 fe20  .P3.m.>....."..
00000130: 20ef beb7 3905 0152 2ce7 bad3 f706 7955   ...9..R,.....yU
00000140: 1d79 0f46 8eb5 2a43 4092 7f0d 8e7c cf02  .y.F..*C@....|..
00000150: 6351 5642 2881 1809 1c88 57c3 1ae6 84ea  cQVB(.....W.....
00000160: d815 bf18 90c1 6f35 8889 40a5 39c7 619f  ......o5..@.9.a.
00000170: abb3 cc83 bd43 7a30 0508 816d 4933 d83e  .....Cz0...mI3.>
00000180: 6540 2348 1f3c 38ce 715a b1b6 b7e5 1616  e@#H.<8.qZ......
00000190: 4ec2 9bb1 11bf 0a2c 7d6a 42c5 c217 1e6d  N......,}jB....m
000001a0: 33a7 395f ddb4 34a9 5da9 88f8 ad7e 124c  3.9_..4.]....~.L
000001b0: 8c5e 5f0a 969c 888a 85e5 6061 f00a 6a7c  .^_.......`a..j|
000001c0: 0421 e689 6651 4190 6a68 e341 b4ee 075f  .!..fQA.jh.A..._
000001d0: e77d 388f 2f40 9f51 827c a7b1 d5a1 58c6  .}8./@.Q.|....X.
000001e0: 4a6b 27f4 25f1 ea9e 6f32 ac1d 9f5b 8185  Jk'.%...o2...[..
000001f0: da5b 5545 4893 d0b4 c56e 3a21 2562 8e0c  .[UEH....n:!%b..
00000200: b464 390f 00eb d084 06bd 4304 aa45 84fa  .d9.......C..E..
00000210: a700 2653 2f5e e30e 1716 0fcf c9fe b55b  ..&S/^.........[
00000220: d6d1 c3cc 9c50 4472 9f14 2539 f297 a8d4  .....PDr..%9....
00000230: abb3 0ac9 da51 5a0c 65b6 e38f 48d9 e707  .....QZ.e...H...
00000240: d305 20c8 187a e1a2 2236 0b92 f8e2 c614  .. ..z.."6......
00000250: c1ad fed7 e19b ff17 7245 3850 904b 1a6d  ........rE8P.K.m
00000260: 43ae 5886 1548 0200 00                   C.X..H...
```

When I saw this, it overwhelmed me and it made me confused what to do, so I reread the challenge description and noticed the *Helpful Reading Material*. I noticed the Hex Dump Article, so I went through it and it made me realize that this `data.txt` file was a hex dump.

So, I went through the list of commands that it suggested and noticed `xxd` to be the most useful for this situation. After reading it's manual, I discovered that `xxd` somewhat translates it's binary data and the text equivalent into hexadecimal text, which can be safely placed or moved into a file.

### Result

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ xxd -r data.txt
␦h��dh␦2.binH��BZh91AY&SYK␦mC����W������������ύ���?�����e:����;0h24hC@d� �h��F����&z��z���H�4h�MM#@�����hQ�#@���dɦ�ф��� @
       L��h␦d���␦hb14
@�␦␦
�|�cQVB(�4�~��Վ��W�␦������o5��@�9�a���̃�Cz�mI3�>e@#H<8�qZ����N��3m�>֜��"�  ﾷ9R,���yUyF��*C@�
,}jB��m3�9_ݴ4�]����~L�^_
������`a�
j|!�fQA�jh�A��_�}8�/@�Q�|��աX�Jk'�%��o2��[���[UEH�д�n:!%b�
                                                          �d9�Є�C�E���&S/^�����[���̜PDr�%9�ԫ�
��QZ
    e��H��� �z�"6
                 ����������rE8P�K␦mC�X�H
```

So, I made a command where it reverses the Hex Dump of `data.txt` and redirects the output into a file.

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ xxd -r data.txt > output
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ ls
data.txt  output
```

With that, I checked the file type of the output file, and noticed it changed.

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ file output
output: gzip compressed data, was "data2.bin", last modified: Sat Sep 26 21:54:21 2026, max compression, from Unix, original size modulo 2^32 584
```

Seeing this, reminded me of the list of commands that the level suggested for this level. 

### Getting to the solution:

Here, I tried using the `gzip` command with `-d` flag to decompress the file.

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ gzip -d output
gzip: output: unknown suffix -- ignored
```

When I saw this, I assumed that it needed the file to be named similar to how gzip files are named, which required it to end with the `.gz`.

So, I renamed it to `output.gz` using `mv` and it changed it's file type.

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ mv output output.gz
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ gzip -d output.gz
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ ls
data.txt  output
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ cat output
␦h��dh␦&SYK␦mC����W������������ύ���?�����e:����;0h24hC@d� �h��F����&z��z���H�4h�MM#@�����hQ�#@���dɦ�ф��� @
       L��h␦d���␦hb14
@�␦␦
�|�cQVB(�4�~��Վ��W�␦������o5��@�9�a���̃�Cz�mI3�>e@#H<8�qZ����N��3m�>֜��"�  ﾷ9R,���yUyF��*C@�
,}jB��m3�9_ݴ4�]����~L�^_
������`a�
j|!�fQA�jh�A��_�}8�/@�Q�|��աX�Jk'�%��o2��[���[UEH�д�n:!%b�
                                                          �d9�Є�C�E���&S/^�����[���̜PDr�%9�ԫ�
��QZ
    e��H��� �z�"6
                 ����������rE8P�K␦mCbandit12@bandit:/tmp/tmp.4hA5LPPiC8$ file output
output: bzip2 compressed data, block size = 900k
```

When, I noticed that it changed to a bzip2 file, so I assumed it had the same requirements. Therefore, I renamed it to have the `.bz2` file type at the end and when I decompressed it, it changed back to gzip.

So, I repeated the process over and over again, until it suddently changed to another type.

```
gfile output
output: POSIX tar archive (GNU)
```

After going through the list of commands again, I noticed this required the command of `tar` which is a sort of archiving tool, that also had a tool to extract the contents of the archived file. 

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ tar -xvf output.tar
data5.bin
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ cat data5.bin
I=4␦d444h������G���2i�24ɑ��!�&�:�� 2�034315256037415011255 0ustar  rootrootBZh91AY&SYu������^@�W��P#�t1��LF@|� �
                                     s
                                      ��zྩ�Do���TW2e�h�O;�GC_]v�'LƆ�Rڝ���nv�v� ��V\��}-��lW�����Z� c�=�M31��g5�T�*ˈY�9�w$S�      �_��bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ file data5.bin
```

The process of decompressing it from either bzip2, gzip, or POSIX tar repeats, until there I fully decompressed it.

### The Solution

```
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ mv data8.bin data8.bin.gz
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ gzip -d data8.bin.gz
bandit12@bandit:/tmp/tmp.4hA5LPPiC8$ cat data8.bin
The password is qQYQiHOBPR8zR61qxYqX45quvihF2uzk
```
### Key Takeaway
This level was tedious and filled with testing. If I could name one thing I learnt from this level is that you should always keep trying in finding a solution and check your available tools because you'll never know what will be useful to you.

### Tools/Commands Referenced
`mktemp` - allows you to create temporary files or directories.
`cp` - allows you to copy a file and place it into another directory.
`mv` - allows you to rename a file.
`xxd` - allows you to convert and translate binary data into hexadecimal text.
`gzip` - allows you to compress files into a gzip file and decompress gz files.
`bzip2` - allows you to compress files into a bzip2 file and decompresses bz2 files
`tar` - allows you to group up files into an archive and extract archives.

<-- [Previous Write-Up](bandit-11-12.md) | [Next Write-Up](bandit-13-14.md) -->
