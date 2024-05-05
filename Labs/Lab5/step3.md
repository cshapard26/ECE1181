---
layout: stepguide
---
# Lab 5: Step 3
[Back to Lab 5 Guide](./guide.md) (Try and use the back arrow on your browser. Clicking this removes your checkmarks)

---
# Explanation
This step asks you to add some seperation between the code section and the `.data` section of your program. You can do this by adding any number of Arbitrary Commands like so:
```ARM
.global _start

_start: MOV R0, #1
    LDR R1, =helloworld
    MOV R2, #{The number of characters in your helloworld line. '\n' counts as a single character}
    MOV R7, #4
    SVC 0

    MOV R0, #0
    MOV R7, #1
    SVC 0

    @ ARBITRARY COMMAND
    @ ARBITRARY COMMAND
    @ ARBITRARY COMMAND


.data
helloworld: .ascii "Hello {your name here}!\n"
```
Some examples of Arbitrary commands are moving a `#0x0` into an unused register, Adding 0 and 0 and putting the sum in an unused register, and similar tasks.

When figuring out how many bytes you added to the program through your Arbitrary Commands, remember that each ARM command is 4 bytes long.

NOTE:
You may remember that the last `SVC 0` statement exits the program. So, it's fair to ask, why are we adding commands after that line? Basically, the makefile will compile the code as it is written. It is not aware that the program ends at the last `SVC 0` statement. However, if you try and run through the program in the debugger, it won't execute the arbitrary commands you added. The reason we are putting them after the `SVC 0` statement is to create a *seperation* between the code and the `.data` section. The only way to do this without getting a compiler error is to add actual commands, even if they won't be executed.

