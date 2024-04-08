---
title: "HW4: Fun for the Whole Family!"
author: Cooper Shapard
date: 2024-04-08
category: Jekyll
layout: post
---
[Download Homework](https://www.coopshap.com/ECE1181/pages/HW4.pdf)


> "I am become ~~Death~~ Homework 4, destroyer of ~~Worlds~~ ~~Grades~~ Mental Health"

> ~ Someone important probably

## Things to Note:
1. **Comment your name somewhere in every question or else you won't get credit.**
2. Your programs can work by taking command line arguments and printing out the answers, but that is not required. Hard-coding the inputs and using the debugger to check the registers/memory for outputs is perfectly fine and will award you full credit.
3. **If you have work down, you will get partial credit.** It doesn't have to work completely, but if we can see that your logic is in the right place, then you will get points back.

## Question 1:
### Code Given
Here is the factorial program from chapter 3, modified to work for your Red Pitaya:
```asm
@ R6 = Input number
@ R7 = Output number

.global _start
_start: 
    MOV R6, #10         @ Load the number you want to factorial, n, into r6
    MOV R7, #1          @ If n <= 0, n! = 1

loop:
    CMP R6, #0
    MULGT R7, R6, R7
    SUBGT R6, R6, #1    @ Decrement n
    BGT loop            @ Keep multiplying until R6 == 0
    BLE stop

stop:
   MOV R0, #0           @ End the program
   MOV R7, #1
   SVC 0
```
### Explanation
Your job is to turn this program into a subroutine/function. It should take 1 input, n, place it into R6 and have one output (stored in R7).

There's 2 ways you can go about this:
1. Put your main program above this code
2. Put this code in a new file, and combine them like you did in Lab 7.

Number 2 looks better and is proabably better coding practice, but Number 1 is easier, so it's probably better for this assignment.

To do this, you must first create a name for the function call. You can call it whatever you would like. Simply replace the _start tag like so:
```asm
.global _start
_start:

    @ Later code goes here


    MOV R0, #0
    MOV R7, #1
    SVC 0

yourFunctionCallName:
    MOV R6, #10
    ...
    etc
```

Now, we need to start filling in the code for the `Later code goes here`. You will need to write the code on your own, but here is your goal in pseudocode:
1. In the _start section, move any number less than 10 (probably between 4 and 8) into a register of your choice.
2. Add that register to the stack like you did in Lab 7. **You must use Empty Descending Stacks or you will lose points**
3. Branch and Link to yourFunctionCallName
4. At the top of the yourFunctionCallName block, before any other code, pop the value off the stack and into R6 like you did in Lab 7.
    a. Note that this line *replaces* the `MOV R6, #10` line. You should not have both.
5. At the end of the yourFunctionCallName block, instead of Branching to stop upon ending, push R7 to the stack and branch back to the Link Register (LR) like you did in Lab 7 (using `BX LR`). Remember to use Empty Descending Stacks for pushing data.
6. Upon returning to the _start section, pop the values off the stack and into a register of your choosing.

Test your program with the debugger to make sure it works properly. I would suggest having the initial value from step 1 be "4" just as a test. Make sure that when your program completes, the value "24" (or 0x18) is in the register you chose for step 6. Make sure to comment your name somewhere in the code and take screenshots of both the code and the program running (showing the debugger with your factorial answer).

## Question 2:
### Code Given
Here is the sine lookup table from Example 12.1 of the book, modified to work with your Red Pitaya:
```asm
@ Registers used:
@ R0 = return value in Q31 notation
@ R1 = sin argument (in degrees, from 0 to 360)
@ R2 = temp
@ R4 = starting address of sine table
@ R7 = copy of argument

.global _start
_start:
    MOV R1, =#theNumberYouWantToLookup(I would recommend 30, which should spit out 0x40000000)

    MOV R7, R1          @ Make a copy of the argument
    LDR R2, =#270       @ Constant won’t fit into rotation scheme
    LDR R4, =sin_data   @ Load address of sin table

    CMP R1, #90         @ Determine quadrant
    BLE retvalue        @ First quadrant? If so, just lookup

    CMP R1, #180
    RSBLE R1, R1, #180  @ Second quadrant? If so, lookup 180 - yourArgument
    BLE retvalue

    CMP R1, R2
    SUBLE R1, R1, #180  @ Third quadrant? If so, lookup yourArgument - 180
    BLE retvalue

    RSB R1, R1, #360    @ Otherwise, it is fourth. Lookup 360 - yourArgument

retvalue:               @ Get sin value from table
    LDR R0, [R4, R1, LSL #2]    @ LSL #2 because each data is 4 bytes
    CMP R7, #180
    RSBGT R0, R0, #0    @ Negate the value if yourArgument > 180

done:
    MOV R0, #0          @ End the program
    MOV R7, #1
    SVC 0

.data
sin_data: .word 0x00000000,0x023BE164,0x04779630,0x06B2F1D8,
                0x08EDC7B0,0x0B27EB50,0x0D613050,0x0F996A30,
                0x11D06CA0,0x14060B80,0x163A1A80,0x186C6DE0,
                0x1A9CD9C0,0x1CCB3220,0x1EF74C00,0x2120FB80,
                0x234815C0,0x256C6F80,0x278DDE80,0x29AC3780,
                0x2BC750C0,0x2DDF0040,0x2FF31BC0,0x32037A40,
                0x340FF240,0x36185B00,0x381C8BC0,0x3A1C5C80,
                0x3C17A500,0x3E0E3DC0,0x40000000,0x41ECC480,
                0x43D46500,0x45B6BB80,0x4793A200,0x496AF400,
                0x4B3C8C00,0x4D084600,0x4ECDFF00,0x508D9200,
                0x5246DD00,0x53F9BE00,0x55A61280,0x574BB900,
                0x58EA9100,0x5A827980,0x5C135380,0x5D9CFF80,
                0x5F1F5F00,0x609A5280,0x620DBE80,0x63798500,
                0x64DD8900,0x6639B080,0x678DDE80,0x68D9F980,
                0x6A1DE700,0x6B598F00,0x6C8CD700,0x6DB7A880,
                0x6ED9EC00,0x6FF38A00,0x71046D00,0x720C8080,
                0x730BAF00,0x7401E500,0x74EF0F00,0x75D31A80,
                0x76ADF600,0x777F9000,0x7847D900,0x7906C080,
                0x79BC3880,0x7A683200,0x7B0A9F80,0x7BA37500,
                0x7C32A680,0x7CB82880,0x7D33F100,0x7DA5F580,
                0x7E0E2E00,0x7E6C9280,0x7EC11A80,0x7F0BC080,
                0x7F4C7E80,0x7F834F00,0x7FB02E00,0x7FD31780,
                0x7FEC0A00,0x7FFB0280,0x7FFFFFFF

```

Here is your new lookup table with values of 0-180 (if this doesn't work, you may need a `.word` tag in front of each line):
```asm
sin_data: .word 0x00000000,0x023BE164,0x04779630,0x06B2F1D8,
                0x08EDC7B0,0x0B27EB50,0x0D613050,0x0F996A30,
                0x11D06CA0,0x14060B80,0x163A1A80,0x186C6DE0,
                0x1A9CD9C0,0x1CCB3220,0x1EF74C00,0x2120FB80,
                0x234815C0,0x256C6F80,0x278DDE80,0x29AC3780,
                0x2BC750C0,0x2DDF0040,0x2FF31BC0,0x32037A40,
                0x340FF240,0x36185B00,0x381C8BC0,0x3A1C5C80,
                0x3C17A500,0x3E0E3DC0,0x40000000,0x41ECC480,
                0x43D46500,0x45B6BB80,0x4793A200,0x496AF400,
                0x4B3C8C00,0x4D084600,0x4ECDFF00,0x508D9200,
                0x5246DD00,0x53F9BE00,0x55A61280,0x574BB900,
                0x58EA9100,0x5A827980,0x5C135380,0x5D9CFF80,
                0x5F1F5F00,0x609A5280,0x620DBE80,0x63798500,
                0x64DD8900,0x6639B080,0x678DDE80,0x68D9F980,
                0x6A1DE700,0x6B598F00,0x6C8CD700,0x6DB7A880,
                0x6ED9EC00,0x6FF38A00,0x71046D00,0x720C8080,
                0x730BAF00,0x7401E500,0x74EF0F00,0x75D31A80,
                0x76ADF600,0x777F9000,0x7847D900,0x7906C080,
                0x79BC3880,0x7A683200,0x7B0A9F80,0x7BA37500,
                0x7C32A680,0x7CB82880,0x7D33F100,0x7DA5F580,
                0x7E0E2E00,0x7E6C9280,0x7EC11A80,0x7F0BC080,
                0x7F4C7E80,0x7F834F00,0x7FB02E00,0x7FD31780,
                0x7FEC0A00,0x7FFB0280,0x7FFFFFFF,0x7FFB0280,    @ 90 degrees is this line's 0xFFFFFFFF
                0x7FEC0A00,0x7FD31780,0x7FB02E00,0x7F834F00,
                0x7F4C7E80,0x7F0BC080,0x7EC11A80,0x7E6C9280,
                0x7E0E2E00,0x7DA5F580,0x7D33F100,0x7CB82880,
                0x7C32A680,0x7BA37500,0x7B0A9F80,0x7A683200,
                0x79BC3880,0x7906C080,0x7847D900,0x777F9000,
                0x76ADF600,0x75D31A80,0x74EF0F00,0x7401E500,
                0x730BAF00,0x720C8080,0x71046D00,0x6FF38A00,
                0x6ED9EC00,0x6DB7A880,0x6C8CD700,0x6B598F00,
                0x6A1DE700,0x68D9F980,0x678DDE80,0x6639B080,
                0x64DD8900,0x63798500,0x620DBE80,0x609A5280,
                0x5F1F5F00,0x5D9CFF80,0x5C135380,0x5A827980,
                0x58EA9100,0x574BB900,0x55A61280,0x53F9BE00,
                0x5246DD00,0x508D9200,0x4ECDFF00,0x4D084600,
                0x4B3C8C00,0x496AF400,0x4793A200,0x45B6BB80,
                0x43D46500,0x41ECC480,0x40000000,0x3E0E3DC0,
                0x3C17A500,0x3A1C5C80,0x381C8BC0,0x36185B00,
                0x340FF240,0x32037A40,0x2FF31BC0,0x2DDF0040,
                0x2BC750C0,0x29AC3780,0x278DDE80,0x256C6F80,
                0x234815C0,0x2120FB80,0x1EF74C00,0x1CCB3220,
                0x1A9CD9C0,0x186C6DE0,0x163A1A80,0x14060B80,
                0x11D06CA0,0x0F996A30,0x0D613050,0x0B27EB50,
                0x08EDC7B0,0x06B2F1D8,0x04779630,0x023BE164, 
                0x00000000
```
### Explanation
This one should go quick. Your job is simply to update the code so that the sine lookup table works for values from 0-180 instead of 0-90. I have already provided the updated lookup table above, so all you need is the code change. I can't do your assignment for you, but here is some psuedocode/questions to ask to guide you:

1. Hint: They say that a good coder can write 10 lines of code in an hour, but a great coder can *delete* 10 lines of code in an hour.
2. The lookup table can handle values from 0-180. Values from 181-360 are simply the negative version of (360 - theValue).
3. Your code already handles these values. You simply need to remove the redundant statements by only checking if the value is above or below 180. If it is above, `RSB R1, R1, #360`, then make the answer negative.
4. Check that your program works by making sure that both 45 and 135 have the same output using the debugger. Also, make sure that the value 255 gives a negative answer (in two's complement).

Take screenshots with a couple different values (the ones in Step 4 are plenty to prove it works properly). And make sure to take a picture of your code **with your name commented somewhere in it.**

## Question 3:
### Code Given
Although there is no code given for this section, here is a basic framework for the assignment. The values (each a word in length) in _valueList are randomized, and your program needs to put them in order. These values can be whatever you would like, but this is probably the simplest way to do it.
```asm
.global _start
_start:


    @ Your code goes here


end:
    MOV R0, #0
    MOV R7, #1
    SVC 0

.data
    _valueList: .word 0x88888888, 0x44444444, 0x00000000, 0x11111111, 0x99999999, 
                      0x77777777, 0x22222222, 0x33333333, 0x55555555, 0x66666666

```

### Explanation
There are many ways to go about this program, but here is how I would do it. This will follow the C code shown in the actual homework instructions, but put in more readable language:
1. Use a register to keep track of the size of the list, which should be 10 (not 0x10). I will call this "the size". To account for the indexes being 0-9, I would start by subtracting 1 from the size. Create another register to keep track of the number of pairs (which I will call "the pairs"). Assign the size's value to the pairs. Also dedicate a register to store the address of _valueList, and another register to store the address of _valueList, but only the second one should be allowed to update (I will call this the updateableValueAddress).
2. Start a loop with a tag like you did in Lab 6. This will be your outer loop. Call it whatever you like.
3. Check if the size is 0. If so, branch to the `end` tag. Otherwise, make the pairs equal to the size - 1 and Branch and Link (BL) to the inner loop, which you will make in the next step.
4. Create another loop with any name you like. This will be your inner loop. It should loop exactly "the pairs" number of times.
5. For each one of the inner loop's loops:
    1. Check if the value stored at the updateableValueAddress is **greater** than the value stored at the (updateableValueAddress + 1). Remember, you are comparing data that is one word long.
    2. If so, switch the two values (a.k.a. store (updateableValueAddress +1)'s value in the updateableValueAddress and vice versa)
    3. If so, also change "the size" to the number of inner loops you have done (this may require some extra math, depending on how you choose to count your loops).
    4. Increment updateableValueAddress. (If each data member is a word, then how many bytes should we incrememnt it?) You can also do this automatically through post/auto indexed addressing if you would like.
6. Once you've done the inner loop "the pairs" number of times, branch back to your link register (which takes you back to your outer loop). Make sure to reset updateableValueAddress to the original value address before you do this.
7. In your outer loop, branch back to the beginning of your outer loop to keep the cycle going. You should already be checking if the size is zero in each outer loop and branching to the end if so.

At this point, your bubble sort implementation should be complete. Test it using your debugger. You should be able to print out all 10 words starting from the _valueList address with the command `x /10uwfx memoryAddressOf_valueList`. Make sure the values are in order (a.k.a. they are listed from 0x00000000 to 0x99999999). Warning: there may be an issue where your program puts 0x88888888 and 0xx99999999 at the beginning of your list. This is because it is interpreting them as negative numbers because of two's complement. You can avoid this by making sure you compare updateableValueAddress and (updateableValueAddress + 1) using an <u>unsigned</u> comparison. For a chart of comparison types/condition codes, see Table 4-1 in chapter 4 of the Smith book.

Take a picture of your memory showing all the values in order. To make sure it works, mix up the values in _valueList and make sure that your program still sorts them correctly. Include a screenshot of your code **with your name commented somewhere on screen** and the memory with the soeted values.

## Question 4:
No sugarcoding: this question is hard. For the people who have coded before, you are making a very efficient map/dictionary here. **Check the [code given](#code-given-3) section, as it gives you free key/data pairs so you don't have to come up with them on your own.**

For those who haven't coded: a map, also known as a dictionary, is a data structure that links a key to a value for efficient memory ans quick lookup. In English, that means that you have 2 lists: one has a bunch of "keys" which are short, and one has a bunch of "values" which are long. Every key is assigned to exactly one value. If you have a key, you can use the "map" to get the value (but not always vice versa). Your job here is to make a map/dictionary that accepts a key as input and quickly displays the value that it is assigned to.

You were given no code nor psuedocode for this question, but I will provide some for you here:
### Explanation
1. Use your program from question 3 to sort the 10 values. It should already do this, so just copy over the code.
2. Modify that code so that when it sorts the keys, it also sorts the data (I have provided you example data in the [code given](#code-given-3) section). Make sure to replace the _valuesGiven with _keys. To do this:
    - Every time you swap 2 keys in your bubble sort algorithm, also swap the locations of the respective data. 
    - Remember: the data is 16 bytes long, and a register can only store 4 bytes. You may need to make multiple swaps to change the location of the data.
    - It may be smart to do this using a macro. If you have learned what that is, great! If not, don't worry about it.
    - A more complex but equally viable solution is to create a list of the memory addresses of the values to represent their ordering, then only swap those. You would likely need to use the stack for this.
3. Test your new code using the debugger to make sure that it works as intended. Use `x /40uwfs memoryAddressOf_values` to print out all the values from memory. The values should be in this order: `0abCdefghijklmno, 1abcDefghijklmno, ... 8Abcdefghijklmno, 9abcdEfghijklmno` (you can tell the correct order by looking at the first number. However, your program should never look at that number to determine the sorting).

That is for sorting the information. Now, for the binary search. Here is a C implementation:
```c
    int low = 0;
    int high = size - 1;
    int mid;

    while (low <= high) {
        mid = (low + high) / 2;

        if (arr[mid] == target)
            return mid;

        if (arr[mid] < target)
            low = mid + 1;

        else
            high = mid - 1;
    }
    return -1; // target not found
```
Let's break that down into more readable psuedocode steps for those with less coding knowledge:
1. Set aside 4 registers. The first should have the key you are trying to search for (should be 0x22222222, or something like that from the _keys list. I will call this "target"), the second should have the number 0 in it (I will be calling this "low"), the third should have the length of your list - 1 in it (in this case, 9, since we have 10 elements total. I will be calling this "high"), and just set aside a register for the 4th (you can just put 5 in it for now. I will be calling this "mid").
2. Start a loop using a tag called whatever you like. This will be your "while loop". It should continue until low is greater than high. Inside the loop, you must:
    1. Add low and high and then divide the result by 2. Put this result into "mid". You may not have learned the divide command, but do you remember the simple way to divide a number by 2? Hint: It involves shifting.
    2. Get the address of the key at position "mid". So, if there are 10 elements in your list and you want the key at position 3, you want the 4th element in the list (we start counting at 0, remember?). You can do this by loading the address of _keys into a register, then adding (mid * theLengthOfYourKeysInBytes). Remember how long each key is? This will get you the address for that element.
    3. Compare the item at that address to the target (you should have a register dedicated to this) If it is:
        - Equal to the target: You've found it! Branch to another part of your program (maybe called "end") where you get the value attached to the target in the same way as step 2.2. So if you grabbed the 4th key, grab the 4th value (remember, the length of the values are different than the length of the keys. Make sure you factor this into your calculations). 
        - Less than the target: Set low to be mid + 1. Branch back to the start of your "while loop."
        - Otherwise: the value must be greater than the target. Set high to be mid - 1. Branch back to the start of your "while loop."
3. This loop will continue until all relevant keys have been checked. If your program has not yet found the key (if it did, it should have branched out of the loop and to the end, where it puts the desired value into either memory or registers then ends the program.), then the key is not in your list. Handle this however you would like (you can also just assume that the key will always be in the map, but this makes debugging harder). I would suggest putting a value like 0x12345678 into a register if th key is not found to signify that it does not exist.
4. Check your program to make sure it works. It should store the correct value in a different part of memory (consider setting aside a word in the .data section) or split up the value and put it in registers (16 bytes total with 4 bytes in each register, so 4 registers total). If you used 0x22222222 as your target, your output should be "2abcdefGhijklmno".
5. **Take screenshots with your name commented somewhere in the code** as well as your map retrieving the correct value for the target key.


### Code Given
The structure should be the same as you've always written. Set up simple "wrapper" code, exactly like that from the [code given](#code-given-2) in Question three, but replace the .data section with this one:

Here is an example `.data` section, with keys and values lined up correctly. The keys are 1 word each, and the values are 16 bytes, aka, 4 words each. To make sure you are updating the whole value and not just the first number, a random letter in the value is capitalized. Those same letters should remain capitalized in the final, sorted set.
```asm
.data
    _keys:  .word 0x88888888, 0x44444444, 0x00000000, 0x11111111, 0x99999999, 
                  0x77777777, 0x22222222, 0x33333333, 0x55555555, 0x66666666
    _values:       
            .ascii "8Abcdefghijklmno",
            .ascii "4aBcdefghijklmno",
            .ascii "0abCdefghijklmno",
            .ascii "1abcDefghijklmno",
            .ascii "9abcdEfghijklmno",
            .ascii "7abcdeFghijklmno",
            .ascii "2abcdefGhijklmno",
            .ascii "3abcdefgHijklmno",
            .ascii "5abcdefghIjklmno",
            .ascii "6abcdefghiJklmno",
```


## For Funsies
I was reading over y'alls homework, and I found out that Dr. Camp removed one of the questions for this assignment. Yay you! If you are interested, here is the question (we had learned the exact same stuff as you at this point, btw):

(30 points) Write an undefined ARM instruction handler that will implement a POP operation without using the LDM or STM instructions. The routine should handle a variable amount of data to be popped from 1-12 words, where register r0 contains the number of words to be popped. Data should be popped into r1- r12 (depending on how many values are popped). The stack pointer should be updated at the end of the operation. Please show your code works with the Red Pitaya by grabbing a screenshot with your name somewhere on the screen.
