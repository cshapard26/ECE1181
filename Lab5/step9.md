---
layout: stepguide
---
# Lab 5: Step 9
[Back to Lab 5 Guide](./guide.md) (Try and use the back arrow on your browser. Clicking this removes your checkmarks)

---
# Explanation
"Wow!" you say, "I've been speeding through this lab. I bet I will get out 2 hours early!" 

And then you got to step 9.

## Code
I really can't help you much on this part, since it's very important that you work through the problem on your own. All I can do is rephrase the instructions to help you better understand the assignment. Also, you'll definently want an ASCII table pulled up, which you can find with a simple Google search. So, here is some psuedocode:

```ARM
.global _start      @ Provide program starting @ address to linker 
                    @ Set up the parameters to retrieve the character and then call Linux to do it. 
_start:    
     mov R0, #0     @ 0 = StdIn 
     ldr R1, =msg   @ address to receive input 
     mov R2, #1     @ only receive one character 
     mov R7, #3     @ read 
     svc 0          @ Call linux to receive input 


@ YOUR CODE GOES HERE. Psuedocode example:
@   1. The number you input through the terminal will be stored in the address at msg.
@      It will only be a single character. Load the address of msg into a register. 
@      Then, load the one-byte character into a different register.
@   2. Check if the character is a capital 'Z' or a lowercase 'a' (your choice which   
@      one to check. You don't have to check both). Make sure to change the CSPR flags
@      with your comparison. You can either do this using the CMP or SUBS commands.
@   3. If the inputted character is greater than a 'Z' (or greater than or equal to an @      'a'), then subtract a constant from that number. You have to figure out this 
@      constant on your own. Hint: if 'a' is 0x61 and 'A' is 0x41, what number would @      you have to subtract to switch between them?
@   4. If the inputted character is less than or equal to a 'Z' (or less than an @      'a'), then add a constant from that number. You have to figure out this 
@      constant on your own. Hint: if 'A' is 0x41 and 'a' is 0x61, what number would @      you have to add to switch between them?
@   5. Make sure that the above two steps are only executed CONDITIONALLY. You can 
@      find the conditional execution keywords in the book.
@   6. Store the new value for the character back in the address of msg (you should 
@      have that address in a register already).
@

                    @ Set up stdout to echo that character back and then call Linux to do it. 
     mov R0, #1     @ 1 = StdOut 
     ldr R1, =msg   @ string to print 
     mov R2, #2     @ length of string (include ‘\n’ to print nicely) 
     mov R7, #4     @ linux write system call 
     svc 0          @ Call linux to print 

     mov R0, #0     @ Use 0 return code 
     mov R7, #1     @ Service command code 1 
     svc 0          @ Call linux to terminate 
.data 
msg:     .ascii "0\n" 

```
## Testing
To checck if your program works, be sure to use the `make` command in the terminal and be sure it compiles without errors. If there is an error in compiling, then the TAs can help you through any issues you may have. Odds are it's just a typo. 

Then, test a few different alphabetic characters to make sure your program works. Simply run `./inputASCII` (or whatever your program's name is), and then enter in a single character (if you input more than one letter on a line, the code will just ignore anything after the first letter). Type a lowercase letter and make sure that it outputs an uppercase version of the same letter. Try an uppercase letter and make sure it outputs the lowercase version of the same letter. If it doesn't, then check that the constant you are adding/subtracting is in Hex or Decimal (for whichever math system you are using). 

Make sure to test "edge cases." If you test the letter 'Z', do you still get 'z'? what about 'a' to 'A'? If the other tests work, but one of these don't, then you may check your conditional execution statements to see if they are "Greater than" or "Greater than or equal to".


## Errors
I tried to add a section about errors and how to fix them to this website, but I couldn't get it done in time. As always, the TA's will be around to help you debug. Because you will be coding your own solutions from now on, I will try and add the Errors and Solutions section by next week.