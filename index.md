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

After successfully running the code, the output looks as follows:
<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/colourclock.GIF"> <br>





### **Simulation**
Vivado also supports and provides simulation designs in software. Simulation is usually done before pushing the code onto the hardware and before generating bitstreams. This however, is a time consuming task and I often found myself waiting a lot as it is a slow process. One issue I ran into was that I never set my TestBench as the top of the heirarchy in my simulation sources. Once I set it as the top I was able to run my simulation. The simulation image below is of the colour cycle program. 

<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/simulation.png">

The simulation  shows the clock at the top, with the reset right below it. The reset is low at first as its active high. This is simulating how the colour cycle works. The simulation output matches the expected timing sequences of the outputed VGA image.


### **Synthesis**
Describe the synthesis and implementation processes. Consider including 1/2 useful screenshot(s). Guideline: 1/2 short paragraphs.
### **Demonstration**
Perhaps add a picture of your demo. Guideline: 1/2 sentences.

## **My VGA Design Edit**
Introduce your own design idea. Consider how complex/achievabble this might be or otherwise. Reference any research you do online (use hyperlinks).
### **Code Adaptation**
Briefly show how you changed the template code to display a different image. Demonstrate your understanding. Guideline: 1-2 short paragraphs.
### **Simulation**
Show how you simulated your own design. Are there any things to note? Demonstrate your understanding. Add a screenshot. Guideline: 1-2 short paragraphs.
### **Synthesis**
Describe the synthesis & implementation outputs for your design, are there any differences to that of the original design? Guideline 1-2 short paragraphs.
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
