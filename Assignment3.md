CIS 3362 Homework #3: Crack DES
Assigned: 9/26/11
Due: 10/10/11

Groups: Please choose groups of four to work on this assignment (minimum 3 students per group)

The goal will be to break the ciphertext given at the end of this assignment. A file with the same contents will also be posted on the course web site. In a week’s time, the first plaintext block will be given, so that you will have one week with matching plaintext and ciphertext information. (This block won’t “giveaway” the critical part of the message though, but can be used to verify if your guess for the key is correct.)

Protocol used to carry out DES
The original plaintext message was written using valid Radix-64 characters only. A list of these characters and their conversions to a 6-bit integer are given on page 613 of the textbook.

The original plaintext file contained exactly 10 Radix-64 characters per line. For each line, these characters were converted to their binary equivalent, yielding 60 bits. Then, four 0s were added to the end of this, creating 64 bits.

These 64 bits are the input to DES.

Then, the 64 output bits from DES were padded with two more 0s, creating a ciphertext of 66 bits.

These 66 bits were then converted back to 11 Radix-64 characters. Thus, the given ciphertext file consists of 11 Radix-64 characters per line.

To help you with the assignment, an implementation of DES is provided.

Key Restrictions
16 bits of the key are restricted as follows (note: the convention being used is that the key bits are labeled from k1 through k64 with the parity bits k8, k16, k24, k32, k40, k48, k56, and k64):

ki = k32+i for the following values of i: 1, 3, 5, 6, 9, 11, 13, 14, 17, 19, 21, 22, 25, 27, 29 and 30.


What To Turn In (Over WebCourses)
Each group should make one submission. Namely, exactly one person from each group should submit the assignment. If there are duplicate submissions, then the first one graded from the group (which will be random) will correspond to the grade the entire group receives for the assignment. Each submission should contain the following:

1) All code used to help break the message.

2) A write-up explaining the process by which you tried to break the message, as well as a summary of the progress made. If the message was broken, reveal the key used in hexadecimal as well as the contents of the message.

Ciphertext to Break
02c9W0//rTI
xmTwWMu4DPA
0h4PVA/K9C8
IJkbg9LZ0B8
9t09o1J4EMc
uXx0Fuipneg
MWcrp3qYvyM
PWpirIH9Tok
h3K4sPOMY8M
3dTPm49bCmw
aWijkeZPtSc
vXBgxl1lPmI
pWOcKlTFbvY
Ovj5SrCdAMI
1+XufuQHNSU
tmeZun4qseE
LyPSpNMYUi0
+jgjejA4nXE
+395+CAoV+c
ERnzzx+JPkc
9PZIpktWVsY
2Sx8i6CdPLI
/WKxO71cwlY
FRRrlqLW9QI
TGTZW0BwlIY
Mh3wzNq3AAY
KjDW3xuTFUc
btJqqu9dOaM
w+Rf9XF05ew
Ssbb+/w0Rqc
T3veMuYZo9s
LcHcxF+nu6c
bT5lieaBTCU
yyLInj15YLE
C1/3rHdBchU
GgBdM1jbLZs
OVsSY4dVqbE
PLsdCQJ4W+M
OBWngte6pHs
HGknhuSXG7M
C0X6pcVNcP0
mPoW1s0IcDQ
zpt/zDT7irg

