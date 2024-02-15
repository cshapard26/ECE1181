---
layout: labscreen
---

# Lab 4
**Instructions embedded at the bottom of this page for your convenience**<br>
[Back to Homepage](..)

---
# Overview

## Organization: A way to gracefully manage your lab
**Warning: Closing this page will clear your checkboxes**

This is not a replacement for the lab instructions. Please only use this as mental scaffolding, as it's not meant to have comprehensive information on the topic.

For a detailed explanation on each step, click the Step Number, which will redirect you to a page with the explanation. Your checkboxes should be preserved if you use the back arrow to return to this page, but you can also control+shift+click the links to open them in a new page.

<input type="checkbox"> Step 1: Read through pages 87-91. (Please actually do this)<br>
<input type="checkbox"> Step 2: Observe Table 5-3 and note how the B, SB, H, and SH letters affect the LDR instruction.<br>
<input type="checkbox"> [Step 3](./step3.md): Read pages 95-96. "Wrap" the given code in the starting and ending code you have seen in previous labs. <br>
<input type="checkbox"> [Step 4](./step4.md): Compile the code from Step 3.**After completing this step, take a screenshot of your code from Step 3 and the terminal showing that it compiles with no errors.**<br>
<input type="checkbox"> Step 5: Run the debugger with your new code.<br>
<input type="checkbox"> Step 6: Check the content of register R1.<br>
<input type="checkbox"> Step 7: Use the command `x /4ubfx TheNumberYouFoundInR1` to check what is stored in the memory address held in R1. **After completing this step, take a screenshot showing the values stored at R1's memory address.** These should be in the reverse order of the values you had in the "mynumber" line because of Little Endianness.<br>
<input type="checkbox"> Step 8: Step over the next line of code and make sure R2 contains the reverse of the value you saw in R1 (aka, the same order as "mynumber").<br>
<input type="checkbox"> [Step 9](./step9.md): Change "mynumber" to your student ID in hexadecimal and disassemble your program. **After this step, take a screeenshot showing the disassembled code. In your lab report, note which memory addresses hold your student ID.** <br>
<input type="checkbox"> [Step 10](./step10.md): Add the code listed in the instructions. NOTE: Read the changes carefully. Some ask you for a Byte and some ask for a Halfword. You will also need to add a few lines that reset the value of R1 to the address of "mynumber". **Take a screenshot of your code.**<br>
<input type="checkbox"> Step 11: Run the code in the previous step. **Take a screenshot of each comparison (there should be 4 comparisons for this part)**. If you can't fit both examples in one screenshot, then that is okay. **Make sure to discuess the differences in your lab report.**<br>
<input type="checkbox"> Step 12: Read the instructions to learn about storing values.<br>
<input type="checkbox"> Step 13: Update the code based on the instructions (adding STR values), and remake the file. **Include a screenshot of the updated values at the effective address. In your lab report, be sure to discuss what happens.**  <br>
<input type="checkbox"> Step 14: Change the code back to how it was before step 10 (I would comment out the lines using an @, because you will need to bring them back for step 15). Add the lines of code specified in the instructions (they should all be grouped together near the top). **No screenshot is needed, but in your lab report, explain what the line** `STR R2, [R1]` **does.**<br>
<input type="checkbox"> [Step 15](./step15.md): Update your code with the code listed in the instructions. If you commented out the lines in Step 14, you can put them back in (and update the LDR lines to STR). NOTE: Read the changes carefully. Some ask you for a Byte and some ask for a Halfword (It's different than step 10).<br> You will also need to add a few lines that reset the value of R1 to the address of "mynumber". **Take a screenshot of your code.**
<input type="checkbox"> Step 16: Run the code in the previous step. **Take a screenshot of each comparison (there should be 4 comparisons for this part)**. If you can't fit both examples in one screenshot, then that is okay. **Make sure to discuess the differences in your lab report.**<br>
<input type="checkbox"> Step 17: **In your lab report, explain why there is no signed STR instruction.**<br>

## Conclusion
- You should have a TON of screenshots. This lab requires the most screenshots by far.
- I hope this lab helped you get a tactile understanding of how memory addressing works in real systems. It is something that will come up often in later labs.
- If you have any more questions, specific or general, make sure to ask a TA.
- If you have any suggestions for this site, email me at **[cshapard@smu.edu](mailto:cshapard@smu.edu).** I made this last night, so I couldn't include as much as I wanted to, but it will be better for next week's lab. Let me know what you want to see/what would help you most!

# Instructions
<object data="Lab4Instructions.pdf" type="application/pdf" width="100%" height="700px">
    <embed src="Lab4Instructions.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="Lab4Instructions.pdf">Download PDF</a>.</p>
    </embed>
</object>



<!-- Credit goes to https://stackoverflow.com/users/2301402/suneel-kumar for the fallback link code --> 


