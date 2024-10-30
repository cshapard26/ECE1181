---
title: "Lab 9: General Purpose Input/Output (GPIO) Pins"
author: Cooper Shapard
date: 2024-10-30
category: Jekyll
layout: post
---

[Download Lab Instructions](/ECE1181/pages/Lab9/Lab9_GPIO_Hackathon.pdf)

# Overview
Welcome to Lab 9: The Final Lab! Here, you will learn how to interface with the General Purpose Input/Output (GPIO) Pins on your red pitaya. You will receive a breadboard, resistors, LEDs, and jumpers (wires). You will then program your red pitaya to turn on the lights following a certain pattern.

# Explanations
## Step 1
### Figure 1
![Figure 1 (let me know if it doesn't show up here)](/ECE1181/pages/Lab9/gpio-diagram.png)

Wire your breadboard according to the picture above. Note the following:
1. The red pitaya has the golden bits facing upwards. This will make the logo on your case "upside-down."
2. The LEDs are diodes, which means you need to make sure you put them in the right way. One of the pins on each LED is longer than the other. *The shorter one should be plugged into the ground line (next to the blue line on your breadboard).
3. You should have 4 wires connecting to the red pitaya. Check to make sure everything is connected.

If you have questions or need further explanations, ask a TA.

## Step 2
Take out the wire connected to GPIO968 and connect it to the 5v on your red pitaya. This is the bottom right pin on the opposite collection of pins. This should turn on the LED. If it does not, try flipping around the LED. If it still does not, find a TA to check for defective parts. Repeat this process for GPIO969 and GPIO970. All your LEDs should be working before you continue.

## Step 3
Run the following command to fix a change in the newer versions of the red pitaya: `overlay.sh classic`

To allow your pitaya to interact with the pins, set up the Linux GPIO API by running the following command: `cat /opt/redpitaya/fpga/classic/fpga.bit > /dev/xdevcfg`

## Step 4
Read through pages 145-147 of Chapter 8 (beginning of Chapter to "Flashing LEDs" for those without page numbers). Remember that the book is for raspberry pis, so the pin numbers will be different than the red pitaya's in Listing 1 above.

## Step 5
Run the following commands to activate the lights from your red pitaya. You will need to do this every time you connect to your pitaya:
```asm
echo 968 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio968/direction
echo 1 > /sys/class/gpio/gpio968/value

echo 969 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio969/direction
echo 1 > /sys/class/gpio/gpio969/value

echo 970 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio970/direction
echo 1 > /sys/class/gpio/gpio970/value
```

If running these commands do not light up your LEDs, ask a TA for assistance.

## Step 6
Read through pages 148-152 in the book ("Flashing LEDs" to "Moving Closer to the Metal" for those without page numbers). Create a file called "gpiomacros.s" from the modified Listing 8-1 in the [code given section](#code-given). Create another file called "main.s" from the modified Listing 8-2 in the [code given section](#code-given). You also need your "fileio.s" from the last lab, provided again as the modified Listing 7-1 below for your convenience. 

## Step 7
Create a fresh makefile with the code in the [code given section](#code-given).

## Step 8
`make` your file and run `sudo ./main` (you may need to enter the password, "root", to run this command). You should see a change in the lights on your board. It may happen very quickly.

## Step 9
In "gpiomacros.s" file's `.data` section, change the "timespecsec" value to add one second between each light flash. I cannot remember whether this is in seconds or nanoseconds, so if you're the first one here, let me know and I'll update the info for other students.

Run `make -B` to force the program to re-make with your new code. Run `sudo ./main` and check to make sure you can see the lights changing slowly on your breadboard.

## Step 10
Now, modify main.s to display the value of R6 in binary on the LEDs. So the first LED is 2^0, the second is 2^1, the third is 2^2. R6 should start from 7 and count down to 0.

**Record a video of your program working on the breadboard and include in in your submission as a .zip file. Also include your main.s code in your lab report.**

## Step 11 - 16
These steps are not required nor encouraged.

## Step 17 (BONUS 10 POINTS ON LAB)
Modify the program so that the value on the lights only increases when you press a button.
WARNING: To date, nobody has been able to get this working (or: nobody has put in the time to get this working).

# Code Given
## Modified Listing 8-1
```asm
@ Various macros to access the GPIO pins
@ on the Raspberry Pi.
@
@ R8 - file descriptor.
@
.include "fileio.s"
@ Macro nanoSleep to sleep .1 second
@ Calls Linux nanosleep service which is funct 162.
@ Pass a reference to a timespec in both r0 and r1
@ First is input time to sleep in secs and nanosecs.
@ Second is time left to sleep if interrupted

.macro nanoSleep
    ldr r0, =timespecsec
    ldr r1, =timespecsec
    mov r7, #sys_nanosleep
    svc 0
.endm

.macro GPIOExport pin
    openFile gpioexp, O_WRONLY
    mov r8, r0                  @ save the file desc
    writeFile r8, \pin, #2
    flushClose r8
.endm

.macro GPIODirectionOut pin
    @ copy pin into filename pattern
    ldr r1, =\pin
    ldr r2, =gpiopinfile
    add r2, #20
    ldrb r3, [r1], #1           @ load pin and post incr
    strb r3, [r2], #1           @ store to filename and post incr
    ldrh r3, [r1]
    strh r3, [r2]
    openFile gpiopinfile, O_WRONLY
    mov r8, r0                  @ save the file descriptor
    writeFile r8, outstr, #3
    flushClose r8
.endm

.macro GPIOWrite pin, value
    @ copy pin into filename pattern
    ldr r1, =\pin
    ldr r2, =gpiovaluefile
    add r2, #20
    ldrb r3, [r1], #1           @ load pin and post increment
    strb r3, [r2], #1           @ store to filename and post increment
    ldrh r3, [r1]
    strh r3, [r2]
    openFile gpiovaluefile, O_WRONLY
    mov r8, r0                  @ save the file descriptor
    writeFile r8, \value, #1
    flushClose r8
.endm

.data
timespecsec: .word 0
gpioexp: .asciz "/sys/class/gpio/export"
gpiopinfile: .asciz "/sys/class/gpio/gpioxxx/direction"
gpiovaluefile: .asciz "/sys/class/gpio/gpioxxx/value"
outstr: .asciz "out"
        .align 2                @ save users of this file having to do this.
```

## Modified Listing 8-2
```asm
@
@ Assembler program to flash three LEDs connected to
@ the Raspberry Pi GPIO port.
@
@ r6 - loop variable to flash lights 10 times
@
.include "gpiomacros.s"
.global _start @ Provide program starting address to linker

_start: 
    GPIOExport pin968
    GPIOExport pin969
    GPIOExport pin970
    nanoSleep
    GPIODirectionOut pin968
    GPIODirectionOut pin969
    GPIODirectionOut pin970
    @ set up a loop counter for 10 iterations
    mov r6, #10

loop: 
    GPIOWrite pin968, high
    nanoSleep
    GPIOWrite pin968, low
    GPIOWrite pin969, high
    nanoSleep
    GPIOWrite pin969, low
    GPIOWrite pin970, high
    nanoSleep
    GPIOWrite pin970, low
    @ decrement loop counter and see if we loop
    @ Subtract 1 from loop register
    @ setting status register.
    subs r6, #1
    @ If we haven't counted down to 0 then loop
    bne loop

_end: 
    mov R0, #0 @ Use 0 return code
    mov R7, #1 @ Command code 1 terminates
    svc 0 @ Linux command to terminate

.data
pin970: .asciz "968"
pin969: .asciz "969"
pin968: .asciz "970"
low: .asciz "0"
high: .asciz "1"
```

## Modified Listing 7-1
```asm
@ Various macros to perform file I/O  
@ The fd parameter needs to be a register.  
@ Uses R0, R1, R7.  
@ Return code is in R0. 
 
.include "unistd.s" 
.equ O_RDONLY, 0 
.equ O_WRONLY, 1 
.equ O_CREAT, 0100 
 
.data 
    S_RDWR: .word 0666 
 
.macro openFile fileName, flags 
    ldr r0, =\fileName 
    mov r1, #\flags 
    ldr r3, =S_RDWR 
    ldr r2, [r3]                @ RW access rights 
    mov r7, #sys_open 
    svc 0 
.endm 

.macro readFile fd, buffer, length 
    mov r0, \fd                 @ file descriptor 
    ldr r1, =\buffer 
    mov r2, #\length 
    mov r7, #sys_read 
    svc 0 
.endm 

.macro writeFile fd, buffer, length 
    mov r0, \fd                 @ file descriptor 
    ldr r1, =\buffer 
    mov r2, \length 
    mov r7, #sys_write 
    svc 0 
.endm 

.macro flushClose fd 
                                @fsync syscall 
    mov r0, \fd 
    mov r7, #sys_fsync 
    svc 0 
                                @close syscall 
    mov r0, \fd 
    mov r7, #sys_close 
    svc 0 
.endm
```

## makefile (Change the 4 spaces between "as" and "ld" to tabs. I will just stare at you awkwardly if you don't do this and say the makefile isn't working):
```asm
OBJS = main.o

ifdef DEBUG
DEBUGFLGS = -g
else
DEBUGFLGS =
endif

%.o : %.s
    as $(DEBUGFLGS) $< -o $@
main: $(OBJS)
    ld -o main $(OBJS)
```

