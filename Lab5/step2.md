---
layout: stepguide
---
# Lab 5: Step 2
[Back to Lab 5 Guide](./guide.md) (Try and use the back arrow on your browser. Clicking this removes your checkmarks)

---
# Explanation
This step asks you to use your code from Lab 1. However, some people no longer have access to that code, so here is an example of that code for you to use. You can name the file whatever you like.
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

.data
helloworld: .ascii "Hello {your name here}!\n"
```

Now, the instructions ask you to print out the memory where the string is located. Do this in the same way that you did the previous lab, with the `x /4ubfx addressInR1` command. Make sure to note the *address* of the memory as well as the length of the block in memory.