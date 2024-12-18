---
layout: home
title: FPGA VGA Driver Project
tags: David Jayakumar
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
* `block_position` is an 11 bit register that stores the current position of the block, and then this value determines where a block goes on the screen. It is an 11 bit value as it leaves headroom and gives extra space for the 640x480 display size.

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
The simulation shows the `clock` at the top. `Reset` is low after as it is an active high meaning only when a condition is reached it will reset the image. The `hsync` signal toggles periodically and has a shorter period compared to vsync. `Vsync` toggles much less frequently than `hsync`, indicating it synchronizes the start of a new frame. When vsync goes low, the system begins rendering from the top-left corner of the display again (row resets to zero). `Vid_on` indicates whether  data is being actively output. When vid_on is high, the color channels (red, green, blue) should display valid pixel values.
<br><br> `row` increments slowly and represents the vertical position in the display grid. Each increment of row corresponds to a new horizontal line of pixels.
`col` increments rapidly within each row, representing the horizontal position of pixels being rendered in that line.
When col reaches its maximum value, it resets to zero, and row increments (this happens at each hsync pulse).
Each high-to-low transition signals the start of a new row, and this aligns with the col signal resetting to zero.

<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/fastsim.png"> <br>


### **Synthesis**
Synthesis followed the same procedure as before. For my modified design it involved generating a new bitstream, and uploading that to my board. Each logical block below is one of the source files such as the clock, vga sync, and my colour stripes file. The multiplexers at the end are the colours that will be written and outputted on the screen. There are 3 inputs which represent the sync, clock and my updated code. The schematic is very similar to the original code except for the fact that there are multiplexers for the 3 colours and not buffers for every single one.


<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/fastcircuit1.png"> <br>
Upon further inspecting the logic block for the updated code, you can see a new circuit with multiple logic blocks in the programe. `Block_position`, and `counter` are driving data and controlling the signals in the system. The signals are being transformed using various components like adders `(rtl_add)`, multiplexers `(rtl_mux)` and also comparators `(rtl_ge, rtl_lt)`. In the bottom half there is a counter system `(counter_reg [15:0])`, which is used being used as a timer and generating outputs based on the current value.


<br><img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/colourstripescircuit.png"> <br>



### **Demonstration**
The custom design was created successfully, and displayed on the Basys3 Board. It showcased my unique pattern which was a combination of colour cycles composed of stripes and columns.

<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/Fastgif.gif">

### **Conclusion**
I got my design to work in the end. It was a rewarding experience and I learned a lot from it. Debugging and problem solving different issues further developed my understandingg of Verilog and hardware design principles and I look forward to implementing them in future and exploring them more in detail.
