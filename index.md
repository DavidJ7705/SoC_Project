---
layout: home
title: FPGA VGA Driver Project
tags: fpga vga verilog
categories: demo
---

Welcome to the blog for my FPGA VGA Driver Project! The goal of this project is to explore the area of System-on_Chip (SoC) design, and to adapt and design an FPGA VGA Driver to display my very own image on a 640 x 480 display.

## **Template VGA Design**
### **Project Set-Up**
The project set-up involves using the Vivado IDE for simulation, synthesis, and implementation. Below is a screenshot of the project environment, which highlights the settings and board which is part of the Artix-7 family.

<img src="https://raw.githubusercontent.com/DavidJ7705/SoC_Project/main/docs/assets/images/VGAPrjSum1.png">

Vivado is an ECAD tool that provides an easy to follow structure through the entire FPGA design process, from using clocks in coding to the bitstream generation.

### **Template Code**
Outline the structure and design of the Verilog code templates you were given. What do they do? Include reference to how a VGA interface works. Guideline: 2/3 short paragraphs, consider including screenshot(s).
### **Simulation**
Explain the simulation process. Reference any important details, include a well-selected screenshot of the simulation. Guideline: 1/2 short paragraphs.
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
