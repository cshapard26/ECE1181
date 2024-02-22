---
layout: labscreen
---

# Lab 5
**Instructions embedded at the bottom of this page for your convenience**<br>
[Back to Homepage](..)

---
# Overview
Literal Pools and Logic are very important topics in ARM Assembly programming. In this lab, you will expereince first-hand how literal pools are handled by the Red Pitaya and how to implement conditional execution in your code. Before you begin, make sure to read over the introduction in the official instructions, as it will give you a broad view of what you will be implementing. As always, if you have questions, be sure to ask a TA.

## Organization: A way to gracefully manage your lab
This is not a replacement for the lab instructions. Please only use this as mental scaffolding, as it's not meant to have comprehensive information on the topic.

For a detailed explanation on each step, click the Step Number, which will redirect you to a page with the explanation. Your checkboxes should be preserved if you use the back arrow to return to this page, but you can also control+shift+click the links to open them in a new page.

<input type="checkbox"> Step 1: Read through pages 92-95. (Please actually do this)<br>
<input type="checkbox"> [Step 2](./step2.md): Reuse your code from Lab 1 with a modern makefile, and use the `x /4ubfx addreddInR1` command to check the contents of the memory at the specified address. **Take a screenshot of the memory location**, and discuss the range of memory of the string in your lab report.<br>
<input type="checkbox"> [Step 3](./step3.md): Add any number of Arbitrary Commands AFTER the last `SVC 0` statement, but BEFORE the `.data` section. Take note of the total number of bytes you are adding.<br>
<input type="checkbox"> Step 4: Use the `make DEBUG=1` command again and check the new memory address of the `=helloworld` command. **Take a screenshot of the new location**. In your lab report, explain if the number of Arbitrary Commands you added lines up with the new location in memory.<br>
<input type="checkbox"> [Step 5](./step5.md): Now, add a very large about of bytes in the same place as the Arbitrary Commands from earlier. You will need at least `4096 Bytes` to successfully get the error message described in the lab report. **Take a screenshot of the error message**, which will occur when you try and *make* the program. Describe in your lab report how you made this occur. <br>
<input type="checkbox"> Step 6: Create a new file called `inputASCII.s`, and fill it with the code in the Code Given section of the instructions. Be sure to update the `makefile` with the new file name. <br>
<input type="checkbox"> Step 7: After using the `make` command, run the program. NOTE: you are NOT running the debugger for this step. You can run the program by entering `./inputASCII` into the terminal. The program then expects you to enter a character, so if it is not doing anything, just type a single letter in the terminal and click Enter. This should print out the same character that you just entered. <br>
<input type="checkbox"> Step 8: You don't need to do anything for this step, but it tells you to be aware that when you are in the debugger, stepping over the first `SVC 0` command will prompt you for a character like in step 7. It may seem like an error, but just enter in the character and click enter, and it will move on to the next line in the debugger like you are used to. <br>
<input type="checkbox"> [Step 9](./step9.md):  <br>
<input type="checkbox"> Step 10: **Take a screenshot of your code**. In your lab report, explain what it does line-by-line. <br>
<input type="checkbox"> Step 11: Test your program by running it without the debugger. You should input 4 tests: <br>
> 1. Your first initial of your first name in lowercase (which should print out your initial in UPPERCASE on the next line). <br>
> 2. Your first initial of your first name in UPPERCASE (which should print out your initial in lowercase on the next line). <br>
> 3. Your first initial of your last name in lowercase (which should print out your initial in UPPERCASE on the next line). <br>
> 4. Your first initial of your last name in UPPERCASE (which should print out your initial in lowercase on the next line). <br>





## Conclusion
- Hopefully, you now have a better understanding of how Literal Pools work within the ARM Processor. Though you likely won't come across the `pool needs to be closer` error during this semester, it is important to keep in mind why this happens for when you work with microcontrollers in the future.
- You also implemented Conditional Execution for the first time in this lab. You will need to use this for most labs moving forward, and it's truly the first step towards industry level Assembly coding.
- This is also the first lab where you are truly implementing a solution all on your own. This will be a recurring theme, so get used to writing your own ARM code!
- If you have any more questions, specific or general, make sure to ask a TA.
- If you have any suggestions for this site, email me at **[cshapard@smu.edu](mailto:cshapard@smu.edu).** I made this last night, so I couldn't include as much as I wanted to, but it will be better for next week's lab. Let me know what you want to see/what would help you most!

# Instructions
<object data="Lab5Instructions.pdf" type="application/pdf" width="100%" height="700px">
    <embed src="Lab5Instructions.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="Lab5Instructions.pdf">Download PDF</a>.</p>
    </embed>
</object>


<!-- Credit goes to https://stackoverflow.com/users/2301402/suneel-kumar for the fallback link code --> 


