# OverTheWire: Bandit Level 09 - 10
Category: Linux Fundamentals | Date of Publish: 10/04/2026 | Difficulty: 

## The Challenge
Similar to the previous challenge, the user is prompted with a data.txt file that contains jumbled, unreadable text, that apparently contains the password of the next level. The goal of this level is to locate this password, which is apparently suffixed with `=`.

## My Approach

### First Attempt
Initially, I thought that I could use a pipeline consisting of `grep` and `sort` but it proved to be ineffective.

### Result
```
bandit9@bandit:~$ sort data.txt | grep "^="
grep: (standard input): binary file matches
```

### Getting to the solution:
There, I tried checking the file type of the data.txt using `file`, which displayed that it's data. So, I looked through the commands that the levels suggest the user to look at and I tried looking into the `strings` command.

After reading it's manual, I felt that it's the best command to use in this current situation, because it's actually used for files like `data.txt` that contain unreadable, binary information. So, I pipelined `strings` with `cat` and came up with `strings data.txt | cat`

### The Solution
```
bandit9@bandit:~$ strings data.txt | cat
e43^3
Ei5,z
 SsT*i
D]M^
:`47eb
6s1$JB
#C)L
[<.-C@}
D4"B
\========== the
y'fX
q:=V
C$5w
DPiP
P6x3
Nf      ;U/
3wEp
Nnm1
86+3_
~z8E
P@7j
BES<0
X~~B
L4Wz
'_$(
yT@&
k#|55qV
{j"v
K       *=
xv<#
)5X8I
/9g}
E5I%
Z#:m
=zZw
nd $:
1jy0m
_TPC!/
h,+$
(JhT
(:Dr
{.`e
i`\Zn
,xQb
^1pJ
        zs)
Gv.|Q(
+D%u$
w=-1
y-j#;
vc.=
        `5w25V
*e8oQ
)X,U2
<Vr:
[FX)
+c7bml
I/tCd
cQoHde
ox8UD
========== password
L9 g5
nNp.
8&o%9
%       \(
fQnR
6xyy<
Ql :
<n6#
x)"k0
gJ      d
iDe\
5VZ(5
O1/k
`T0DWA
        Bs$
NQJ|v
v_6i1xu[
tee'>
eN6N$
W^-O
D       Z1)5A
        <WvJ
\8Ln
<u*========== is
E!aV
=9$s
6YgE
G]u/
!/W)
5k'[
awaOpX
m'K5
M,HS
EHR@E
%^'^
Nz.l
P%(i!
;D04
c*@<
yU4E
A\<>
VC6i
H)FH0Y
6GRp/
;R#/dr
J%3v
aa:b'
fw6cs^
,V3`
zy:D
E8Cd
K<Q]
#>h|
vccgw
oi[v
5)S6
s('1
Gv(^I
G2un.
mfD
l]X kW
{=S[
jV+^
DAUKh
Bu_"M
Bp&;\h
h&>
/0~c
v:{oT
D"rH+
?w5f
m4      o<860S
>v9-
K?v]
Tu[9U
;a =
1[z1
DE.I
39:.
,F/HxY
zl:nd
*{18
Ox6:
-vX4
a#B"
KX&~
$3-L
l<`e
I#|0M|W
V|l`
VVw7
KC{j
li6r
*~pN2
$OVk|
lp>IoY-b==ld
uJb[
{<<f
6r-V
[i(I
]|,r
0`'l
s(3Y8
.%>u
?I_^B
bXn5
vdsM#bc
FRhU
cc,J
%fX2
b_]C
rnq<
d\65
u4HO
*]6~
b9Np)
p,].*
Bs;B
/DkG
%!zi
C;; mX
        >hL
q#~W
T-We
E-i17
!0AL
{y<Y
;8Z@)<$,
.y(gXP
NhF :
========== B0s2khmbT9u0geKuOoVGW3JZKhndE3BG
ec=}
```

### Key Takeaway
Each level I face, I find a new command that I'm amazed by. I believe this goes to show how I'm always gonna learn something new whenever I encounter different and challenging problems along this journey.

### Tools/Commands Referenced
`strings` - extracts human, readable text contents from files
`cat` - displays file contents

<-- [Previous Write-Up](bandit-08-09.md) | [Next Write-Up](bandit-10-11.md) -->
