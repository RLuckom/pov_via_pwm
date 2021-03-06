# Persistence of vision project using TLC5940 grayscale LED driver and BeagleBone Black

One of my good friends has a masive NYE party every couple of years. This year the theme was "lights," and guests were encouraged to have costumes or cool stuff related to the theme. I got a BBB in November with a plan to make a massive, ~760 light display. I really wanted to learn to use the BBB's two Programmable Realtime Units, and I also needed grayscale control of the LEDs, for which the only good through-hole option seemed to be TI's TLC5940 LED drivers. After a lot of work I realized that I didn't have enough time to finish that project before New Years. Thanks partly to  [this post] [0], I found that I could use a lot of the work I'd done to construct a simple persistence of vision display.

This is one of my first electronics projects, and I had to learn about a lot of different things. In almost every case, my choices were motivated by the desire to learn and understand exactly how and why everything worked, rather than aiming for efficiency or architectural quality. I'm very pleased with the way this turned out--often if I choose a project with a too-steep learning curve I never finish it at all--but there are many things about this project that still need substantial work.

After I had successfully [set up the PRU pins][1] and run the [examples][2], I decided to try to make a simple assembly program to bit-bang the TLC5940 grayscale cycle. I was very lucky to find an [open-source book][3] describing the protocol to use and linking to a [programming flow chart][4] published by TI but inexplicably hard to find via the documentation for the TLC5940. Using these two resources and TI's [PRU-ICSS Reference guide][4], I started working on an assembly version of the protocol.

The way the PRUs work from the BBB is that you write a program for the PRU(s) in assembly, then write a C program that runs on the BBB main processor that loads the compiled binary of the assembly program into the PRU, starts it running, and does any necessary communication via interrupts. The examples provided on github demonstrate the project structure and include makefiles to compile the assembly programs along with the C and warn about errors. While I had a fair amount of confusion once I started trying to actually program in assembly, I really appreciated having simple project templates and a one-step build process.

The PRU instruction set is pretty small, consisting of 40 or so different instructions, and the provided compiler also allows the definition of macros and structs. From a programming standpoint I found the hardest part was structuring the code nicely, and I'd still like to refactor more to get the modularity I'd like. Macros seem able to do about 75% of what I'd like from functions, and what I ended up with is, honestly, covered in red sauce and frequently paired with meatballs. I am also indebted to the authors of the prudebug project on sourceforge, without which I'd have been stuck with my original debugging methods involving LEDs, transistors, goat blood and pentagrams.

This was an extremely fun project, and I really enjoyed having a small-ish test project to get me started with the TLC5940 drivers. I'm going to use what I learned in this project toward the original idea I had, about which more (hopefully) soon.

<iframe width="420" height="315" src="//www.youtube.com/embed/lpmPm4T6jZI" frameborder="0" allowfullscreen></iframe>

[0]: http://ch00ftech.com/2011/10/24/led-persistence-of-vision-toy/
[1]: http://derekmolloy.ie/gpios-on-the-beaglebone-black-using-device-tree-overlays/
[2]: https://github.com/beagleboard/am335x_pru_package
[3]: https://sites.google.com/site/artcfox/demystifying-the-tlc5940
[4]: http://mythopoeic.org/BBB-PRU/am335xPruReferenceGuide.pdf
