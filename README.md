# MIXAL programming
## MIX computer structure
### registers
- rA
- rX
- rJ
- rI1
- rI2
- rI3
- rI4
- rI5
- rI6
### flags
- cmp
- overflow
### i/o
- magnetic tape (number : 0 ~ 7)
- disks and drums (number : 8 ~ 15)
- card reader (number : 16)
- card punch (number : 17)
- line printer (number : 18)
- paper tape (number : 19)
### memory
- 4000 CELLS (number : 0 ~ 3999)
- word : 1 sign + 5 bytes
- one cell can store

|sign|1|2|3|4|5|
|---|---|---|---|---|---|
|+/-|byte|byte|byte|byte|byte|
|+/-|6 bits|6 bits|6 bits|6 bits|6 bits|
|+/-|111111|111111|111111|111111|111111|
|+/-|63|63|63|63|63|

## MIX computer character set
|line\column|1|2|3|4|5|6|7|8|
|---|---|---|---|---|---|---|---|---|
|1|0 SAPCE|1 A|2 B|3 C|4 D|5 E|6 F|7 G|
|2|8 H|9 I|10 TRIANGLE|11 J|12 K|13 L|14 M|15 N|
|3|16 O|17 P|18 Q|19 R|20 SUM|21 EXIST|22 S|23 T|
|4|24 U|25 V|26 W|27 X|28 Y|29 Z|30 0|31 1|
|5|32 2|33 3|34 4|35 5|36 6|37 7|38 8|39 9|
|6|40 .|41 ,|42 (|43 )|44 +|45 -|46 *|47 /|
|7|48 =|49 $|50 <|51 >|52 |53 ;|54 : |55 '|
|8|56 |57 |58 |59 |60 |61 |62 |63 |
## MIX computer instruction set
### control the amount of instrution operator, keep common used, reduce spare, extra, useless.
### loading instruction operators
- LDA
- LDX
- LDi

### storing instruction operators
- STA
- STX
- STi
- STJ
- STZ

### arithmetic operators
- ADD
- SUB
- MUL
- DIV

### assigning value operators
- ENTA (spare, extra, useless)
- ENTX (spare, extra, useless)
- ENTJ (spare, extra, useless)
- ENTi (spare, extra, useless)
- INCA
- INCX
- INCJ
- INCi
- DECA (spare, extra, useless)
- DECX (spare, extra, useless)
- DECJ (spare, extra, useless)
- DECi (spare, extra, useless)

### comparison operators
- CMPA
- CMPX
- CMPi

### jump operators
- JMP
- JSJ
- JOV
- JNOV
- JL
- JE
- JG
- JAN (spare, extra, useless)
- JAZ (spare, extra, useless)
- JAP (spare, extra, useless)
- JXN (spare, extra, useless)
- JXZ (spare, extra, useless)
- JXP (spare, extra, useless)
- JiN (spare, extra, useless)
- JiZ (spare, extra, useless)
- JiP (spare, extra, useless)

### miscellaneous operators
- SLA, SRA
- SLAX, SRAX
- SLC, SRC
- MOVE 
- NOP
- HLT

### input-output operators
- IN
- OUT

### conversion operators
- NUM
- CHAR

## MIXAL
### MIXAL pseudo-operation
- EQU
- ORIG
- END
- CON
- *+3 (self plus three)
