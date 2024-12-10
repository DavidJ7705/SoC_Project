---
layout: home
title: FPGA VGA Driver Project
tags: fpga vga verilog
categories: demo
---

Welcome to my FPGA VGA Driver Project! The goal of this project is to explore the area of System-on_Chip (SoC) design, and to adapt and design an FPGA VGA Driver to display my very own image on a 640 x 480 display.

## **Template VGA Design**
### **Project Set-Up**
The project started by setting up a Vivado project and configuring the Basys3 development board for the FPGA programming. Then I had to plug in my board to the computer using a micro USB cable, and also to the monitor using the VGA interface. This lets us run simulation, synthesis, and implementation once the board is programmed. I then added all the files for VGASync.v, VGAColourCycle.v and VGATop.v to the design sources. I made sure all the sources were in the right places for design sources and simulation sources. <br>I worked on a local folder in my machine as there was a permission issue with one drive. I made sure to save a backup of my project to my onedrive as the files had a chance of getting wiped every week.
<br>Below is a screenshot of the project environment, which highlights the settings and board which is part of the Artix-7 family.

<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/VGAPrjSum1.png">

### **Template Code**
#### **Colour Cycle**

The template Verilog design code for a Colour Cycle was used first to get an understanding of the VGA designs. The the clock timing was obtained from the IP catalog under the clocking section.<br> 
<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/clocking .png"><br>
The example code included VGATop.v, VGASync.v and lastly VGAColourCycle.v, which are all design sources. Testbench.v was provided which was used for the simulation sources. The Basys3_Master.xdc was also provided as it is needed to run code on the Basys3 development board. After adding all the files in the correct places, the project flow should look as follows:


<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/templateflow.png"><br>


The code was a simple program which used a state machine to cycle through 8 different solid colours, one after another, on the VGA display. The colours were black, red, yellow, green, cyan, blue, magenta, and white. These all had their own states that get the colour to show up using different 12 bits for colour assignment (colour <=12'b000000001111;).
<br><br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/Colourcyclecode.jpg"> <br>







### **Simulation**
Vivado also supports and provides simulation designs in software. Simulation is usually done before pushing the code onto the hardware and before generating bitstreams. This however, is a time consuming task and I often found myself waiting a lot as it is a slow process. One issue I ran into was that I never set my TestBench as the top of the hierarchy in my simulation sources. Once I set it as the top I was able to run my simulation. The simulation image below is of the colour cycle program. 

<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/simulation.png">

The simulation  shows the clock at the top, with the reset right below it. The reset is low at first as its active high. This is simulating how the colour cycle works. The simulation output matches the expected timing sequences of the outputed VGA image.


### **Synthesis**

After running the simulation successfully, the next step is running synthesis and implementation. Synthesis is a straightforward process which maps the code design to a library of digital gates available in the FPGA. These gates match the functionality of the design.



Implementation assigns logic elements to specific locations on the FPGA board. Connecting these logic elements will allow the board to meet the correct timing and area constraints. The result of the implementation is a bitsream file, which is then used to program the FPGA hardware.


<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/circuitclockcycle.PNG">

### **Demonstration**
After successfully running the code, the output looks as follows:
<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/colourclock.GIF"> <br>
As you can see, the project was a success as it cycled between each colour and reset as intended.



## **My VGA Design Edit**
My initial idea was a to have an image that would have random colours appear on the screen and flow in a satisfying way. It would look similar to the image below.
<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/initidea.gif"> <br>
However, this idea was abandoned as I could not correctly get the right timing, while also making the squares to appear at the right place and right time, and to change in size an colour correctly. If i had more time I would have tried different things to achieve this.

<br>
I decided upon a simpler design which would incolve moving elements, while also adding in intricate shapes and designs. I created an image that had multiple diagonal lines moving across the screen in the colours red, green, and blue.


### **Code Adaptation**
With the help of OpenAI, I changed the original code in the ColourCycle.v file to create and design my own image. This was a challenge at first as I had to learn what each aspect of the code did so I could get an understanding of what was going on.
* `red_reg`, `green_reg`, and `blue_reg` are all 4 bit registers that represent the colour values in my image.
* `block_positio`n is an 11 bit register that stores the current position of the block, and then this value determines where a block goes on the screen. It is an 11 bit value as it leaves headroom and gives extra space for the 640x480 display size.

* `always @(posedge clk or posedge rst) begin` means that this block of code will be triggered when the clock signal is positive, or when the reset signal is positive.

* `if (rst) begin
    block_position <= 11'd0;
end`. This starts the image at the furthest left position. When the reset signal is high, it then resets the block position to the start (Leftmost postion).
* `else begin
    block_position <= block_position + 1;`. When the reset is not activates, the block will move to the right one position at a time, on each clock.
* `if (block_position >= 11'd640)
    block_position <= 11'd0;`. If the blocks position reaches or exceeds the decimal value of 640, it wraps around to the start at the left.
<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/fastcode1.png"> <br>
---


* `if (col >= block_position && col < block_position + 11'd50) begin`. This checks for if the column is within the horizontal range of screen, and if it is, it spans 50 pixels. If the column is also in this range, the code determines the colour of it based on its position.
* `case ((block_position / 50) % 3)`. This determines each block colour by dividing it by 50 and then using the modulo operator to alternate between 0, 1 and 2. These values link to the different registers, for red, green, and blue.
* `Case 0`. The block is fully red when `(block_position / 50) % 3 == 0`.
* `Case 1`. The block is fully blue when `(block_position / 50) % 3 == 1`.
*  `Case 2`. The block is fully green when `(block_position / 50) % 3 == 2`.
* `Default case`. If an unexpected error happens, the block will change to white. All colour registers are set to high as this will output a white.



<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/fastcode2.png"> <br>

---

* `else begin
    red_reg = 4'b1111; 
    green_reg = 4'b1111; 
    blue_reg = 4'b1111; 
end`. If the current pixel is not in the range, it sets the background colour to white. This is part of the if-else block for incrementing by 50 pixels.
* `assign red = red_reg;
assign green = green_reg;
assign blue = blue_reg;`. The assign statements map the internal registers (red_reg, green_reg, blue_reg) to the external signals (red, green, blue). Using assign ensures the output signal will always reflect the values located in the registers. 


<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/fastcode3.png"> <br>
--- 
The actual output of the image moves very fast so it is quite difficult to see what is going on with the pixels. By adding a counter to the code, we can adjust the speed of it and see what is going on. 
* `counter <= 16'd0;`. This resets the counter. It is also 16 bits. This is useful as it is a large range which is necessary for counting clocks and cycles.
* `counter <= 16'd100;`. This means that after every 100 clocks, it resets the image. This lets us see whats happening with each colour block. 
<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/slowcode1.png"> <br>
After slowing down the image it is interesting to see that there is in or around 13 blocks in each diagonal line. This fits with the formula of dividing 640 by 50 as it comes out as 12.8.

<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/Slowgif.gif"> <br>
<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/slowss.png"> <br>


### **Simulation**
Show how you simulated your own design. Are there any things to note? Demonstrate your understanding. Add a screenshot. Guideline: 1-2 short paragraphs.
### **Synthesis**
Describe the synthesis & implementation outputs for your design, are there any differences to that of the original design? Guideline 1-2 short paragraphs.



<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/fastcircuit.png"> <br>



### **Demonstration**
The custom design was created successfully, and displayed on the Basys3 Board. It showcased my unique pattern which was a combination of colour cycles composed of stripes and columns, overlayed on a static image.

<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/Fastgif.gif">

 
 
(insert images of it) 
explaining whats going on
It is a solid colour background, which cycles through different colours separated by stripes and columns, in a clockwise motion around the display.

### **Conclusion**
If you get your own design working on the Basys3 board, take a picture! Guideline: 1-2 sentences.

## **More Markdown Basics**
This is a paragraph. Add an empty line to start a new paragraph.

Font can be emphasised as *Italic* or **Bold**.

Code can be highlighted by using `backticks`.

Hyperlinks look like this: [GitHub Help](https://help.github.com/).

A bullet list can be rendered as follows:
- vectors
- algorithms
- iterators

Images can be added by uploading them to the repository in a /docs/assets/images folder, and then rendering using HTML via githubusercontent.com as shown in the example below.

<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/VGAPrjSrcs.png">
