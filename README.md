Final-Project
=============

Implementation of a stereo video module with Atlys Spartan 6 FPGA.


#Prelab

The final goal of this project is to connect a stereo video module to my FPGA and display it to a VGA monitor.  I will then use a UART terminal to apply different filtering techniques to the video output.  Ideally, I would like to implement an edge-detecting filter, a color-shifting filter, possibly a negative filter, and attempt some form of morphology filter.

- Required Funtionality: Display video and connect to UART.
- B Functionality: Implement 1-2 different filters, including edge-detection
- A Functionality: Implement an additional 1-2 filters, including morphology

My goal is to acheive required functionality by at least the third lesson.

Each subsequent funtionality will have two lessons to complete.

The initial design for displaying video I am taking from a code template provided with the video component.  The challenge here will be making it work on a a much newer version of ISE design suite than it was designed for.  I will examine the feasibility of implementing filtering in hardware vs. software as I go, and possibly take a mixed approach to this problem.  I expect to implement the majority in software, but I would like to take full advantage of the speed of the FPGA.

I have all necessary code templates to begin work.

#Implementation

