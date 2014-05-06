Final-Project
=============

Implementation of a stereo video module with Atlys Spartan 6 FPGA.

Check out this demo video to see the final product:

##(Functionality Update)
I believe my final design has acheived A functionality, with some redefining of my initial plan.  My final design implemented 4 image filters, as well as allowing the altered and unaltered video streams to be displayed side-by-side on the monitor.  In order to focus on the video filtering part of the project, I chose not waste time implementing microblaze again just to read in commands to switch the video filters.  Instead I used the switches provided on the board.

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

The template code I found from Digilent was relatively easy to make work.  However, the code itself was much more complex than I expected.  This made it much more difficult to find and correctly alter signals to implement my filters.  The schematic below shows the complexity of this design.

![Signals, signals everywhere](https://raw.github.com/micfloy/Final-Project/master/schematic.png)

##Difficulty Level Problems Encountered
Because of the complexity of the frame buffer module and the timing, I began by identifying the data bus outputting the pixel values to the DVI transmitter. I then began to break this signal up and implement individual pixel filters.  I spent some time researching each of the filters I wanted to implement.  I soon realized that the more difficult filters I had thought about implementing required a huge leap in my understanding of the template code.  Primarily, how to get specific pixels out of the frame buffer and how to deal with timing problems.  The fact that this code has well over 10 different clocks used by different components made it incredibly difficult to deal with timing problems.  After discussing the problem with Dr. York, it became apparent that in order to even have a hope of implementing 2D array filters, such as edge-detection, I would need at least a second frame buffer and some way to pull specific pixels out of the first buffer to do 8-pixel filters.  This is well beyond the scope of what I have learned about dealing with memory and signals.

##Final Filter Design
After recognizing the problems discussed above, I decided to focus on implementing the single-pixel filters I could find. My final design implements 4 different filters, assigned by combination logic based on the boards switches. 

-Sepia 
    The Sepia filter can be turned on by setting the first three switches on the board to "001".
This filter requires adjusting all three rgb values at the same time.  The red values are increased by about 50 out of 255.  Green is lowered by 15 and blue is lowered by about 56.  Of course, all values had to be given edge conditions to prevent any rollover for very large or small rgb values.  The final effect is a somewhat brownscale image with much stronger red hues

-Negative
    The Negative filter can be turned on by setting the switches to "010".  Probably the easiest filter to implement, the Negative filter only required to break the signal into the three rgb values and then invert them.  Even though it is an incredibly simple filter, it produces some of the most interesting images.
    
-Cutoff
    The Cutoff filter can be turned on by setting the switches to "011".  This is the most creative filter I implemented and also produces some very interesting images.  Any r, g, or b value that is above a given threshold (I used 200) is set back to '0'.  This results in many solid colored surfaces taking on strong pastel colors as the primary color in the object is removed.  Very bright objects turn completely black and their edges take on the same strong colors as one or two of the colors are removed first

-Bright
    The Bright filter can be turned on by setting hte switches to "100".  This filter simply increases the value of all color signals, resulting in a brighter image.  While not being particularly exciting, I wanted to explore this further and implement several different brightnesses.  However, due to the timing errors I will discuss below, I chose not to implement additional filters if they did not demonstrate something new in my design.
    
##Timing Errors

As discussed above, I quickly realized that timing was something I had very little control over in this project because of the complexity of the design I was working with.  Because my filters were being implemented asynchronously, the more complex they became and the more possible filters I implemented, the more timing problems I introduced into my design.  This results in the monitor occasionaly going black and reseting.  I tried to limit this by keeping my filters as fast as possible, with minimal mathematical calculations involved in each step.  I also capped my filters at 4 because when I went much beyond this number the timing became a very big problem.  I made multiple attempts to synchronize all of the outputs to my DVI transmitter, (including the `v_sync` `and h_sync` signals) but could never eliminate the screen reset problem. Eventually, C2C Busho recommended I alter my pixels before they entered the frame buffer.  I made this change to my design and it helped marginally with the screen issues, but they still exist.  What this did allow me to do was display a filtered image on one side of the screen from camera B while displaying an unfiltered image from camera A at the same time.

#Conclusions

This lab taught me a lot about how problems can change a lot over time.  Initially I had some idea of what my difficulties would be, but they turned out to be very different than I expected.  I am quite pleased with the results of my project, but I would have really liked to have some of the key knowledge required to take my design to the next level where I could have done so much more with my video output.  I knew about timing issues before this project, but it was interesting seeing them in action where they created problems, without completely stopping my design from functioning.  This has made me want to get a much better understanding of these concepts and how to combat them in any future designs I work on.

I feel that this project was a really interesting way to end the semester by tackling a piece of hardware that I had never worked with and implementing freely available code with my own modifications to add significant functionality.

#Documentation

My template code was taken from the Digilent website.
C2C Busho advised me on a change to my code that did slightly improve my timing errors, though it did not eliminate them.






