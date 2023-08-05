Amstrad Z80 machine code notes
==============================


1. You can't load a value from a register to a memory address directly using LD.
You can do the opposite, load a register from a memory address.

2. You cant do LD (HL),BC. So how to store a 16-bit register to a memory address?
```
ORG &9c40

LD IX,&9C70
LD BC,&FFFE

LD (IX+0), B
LD (IX+1), C
RET	
```

3. How to add 2 16-bit numbers together?
```
ORG &9c40

LD IX,&9C70
LD HL,0
LD BC, 1
LD DE, 2

ADD HL,BC
ADD HL,DE

LD (IX+0),H
LD (IX+1),L
RET
```

or skip BC and just use two registers storing the value to HL.

```
LD Hl,1
LD DE,2
```
# Do a 'for loop' from 0 to 10. We increment a memory address with a value of 0, ten times.
```
org &9c40

ld a,10
ld b,0
ld hl,&9ca0

loop:

	cp b
	jp z,loop_exit

	inc (hl)
	inc b
	jp loop	
loop_exit:
  ret
```
# do a for loop, printing 0123456789 to the screen.
```
org &9c40

ld a,47        ; 47 is one character before '0'. 
ld hl,&9ca0    ; we load HL with the address to put '47' 
ld (hl),a      ; we load the address pointed at HL with 47


ld a,10        ; loop 10 times
ld b,0


loop:

	cp b
	jp z,loop_exit

	inc (hl)
	ld a,(hl)
	call &bb5a

	ld a,10
	inc b
	jp loop	
loop_exit:
	ret
```

# Print '=' if two 8-bit integers are equal and '<>' if they are not.


```
org #9c40

PrintChar equ #bb5a
ld a,7
sub 7
jp M, flag

ld a,'='
call PrintChar

ret

flag: 
	ld a,'<'
	call PrintChar
	ld a,'>'
	call PrintChar
	ret
```
