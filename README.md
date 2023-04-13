# lab6

UART Lab Experiment

Goals of Lab
Explore Example code from TI and modify it to use the Backchannel UART to communicate with your computer.
Observe UART Communication on the scope.
Run an example UART Print driver. 
Create software which controls the brightness of your LED with UART.

UART Example Code
https://dev.ti.com/tirex/nodeContent?node=A__ALXTttnffpSeB73nkJ2g5g__msp430ware__IOGqZri__LATEST 

Download or import this code into Code Composer and run it on your board.
You should notice “nothing happening”
Observe which pins the current example is using and which eUSCI peripheral is tied to them.
 P4SEL0 |= BIT2 | BIT3; 

Looking at the Launchpad Datasheet, which pins are used for the Back Channel UART and which peripheral is being used?
https://www.ti.com/lit/ug/slau680/slau680.pdf?ts=1680128636524&ref_url=https%253A%252F%252Fwww.google.com%252F 

TXD>> Backchannel UART: The target FR2355 sends data through this signal. The arrows indicate the direction of the signal.

RXD>> Backchannel UART: The target FR2355 receives data through this signal. The arrows indicate the direction of the signal.



Modify the example code to utilize the correct pins for the back channel UART and the correct peripheral.
Hint: You will also need to most likely change the interrupt vector to ensure it is pointing to the correct peripheral.
Open your “Device Manager” on Windows and see which COM Port the “Application UART” is mounted to in your computer.
The MSP430 Launchpad mounts 2 COM Ports, one for Debug and one for the Backchannel UART.
COM 3 (back Channel)

COM 4 (Debug)


In Code Composer, open the “Terminal” window, and click the icon which is called “Open New Terminal”.
Pick the COM Port found for the Application UART
Set the Baud Rate to 9600
Open the Terminal

Once you have modified the example code to use the backchannel UART, run the code, and when you type into the Terminal, you should see those characters come back on the screen.
It may be delayed with your operating system.
This is actually your microcontroller talking back to your computer.
To prove this is actually your microcontroller, MODIFY the example code to add 1 to the received character.
Per the ASCII Table, this will take an input like ‘a’ and present the next letter in the ASCII Table, ‘b’.
https://www.asciitable.com/ 
Test out this modification by typing in letters or numbers and see what characters are returned. 
      UCA1TXBUF = UCA1RXBUF+1;
Observe UART Communication
Turn on your scope.
Using scope probes or BNC to Clips:
Connect the positive probe or measurement probe to the TXD Header above the microcontroller.
Yes, you can connect to those without causing damage.
Make sure that you do not cause the jumper to disconnect while connecting your probe.
Connect the negative probe or ground lead to GND on the dev board.
PLEASE BE CAREFUL OF WHAT THE GROUND PROBE TOUCHES. It can cause short circuits and damage your board.
Running your code from the previous part, you should see the voltage idle at 3.3V, not 0V. 
Press some buttons on your keyboard in the Terminal Window and see if you can see some signals fly by on the scope. 
This is going to be an issue if we want to see an individual UART Transaction right?
Press SINGLE on the top right of the scope.
It should remain YELLOW, meaning it is waiting for a trigger condition.
Press a key on your keyboard, and your scope should see the change and run a single acquisition, holding it on the screen.
The STOP/RUN button should be RED indicating that the scope is currently stopped taking acquisitions. 
What you should see should be a sequence of square waves which look somewhat grouped together. This is your UART message being sent from your microcontroller back to your PC.
To make it where we do not have to keep hitting SINGLE, instead, press the [Mode/Coupling] button on the left side of the control panel. 
You should then see an option for {Auto}.
Using the main select knob (the one with the green arrow above it), change that to {Normal}.
This causes the scope instead of automatically updating the measurement buffer, it instead only does so when it sees the trigger condition.
Press keys on your keyboard and you should see the scope update each time you press a key.
To decode the UART protocol, press [Serial] on the right side of the scope.
Set the protocol to {UART}.
Check the {Bus Config} and ensure that:
{Polarity} is set to “Idle High”
{Baud Rate} is set to “9600”
Now press some more keys, and you should see some HEX numbers begin showing up below the data you are capturing.
To change the number base, in the [Serial] Menu, press {Settings}
Change the {Base} to ASCII
You should now see the characters your device is sending back.

UART Print String
Grab the following files from this repo: https://github.com/Russty32280/RU-Intro-Embedded-Systems-Libraries/tree/main/UART%20Driver%20FR2355 
main.c
FR2355_UART_Driver.c
FR2355_UART_Driver.h
Run the code and observe the output on the Terminal.
MODIFY the code so that the “text” is: char text[] = {‘x’, ‘!’};
Note: The braces are being used to change the text from a string, to now being a character array.
Run the code and observe the output.
What is going on with the result?
Hint: How are “strings” interpreted and represented in C?
Hint: What is the Serialprint function actually doing (yes, those are pointers).


Coding Task: UART Controlled LED
You need to make a piece of software that will set the Brightness of an LED based on your UART input. The input will be the characters “0” through “9”, where “0” should effectively be the LED off, and “9” should be maxed brightness. utilize the value taken in UCA1RXBUF to control the duty cycle of the LED. Utilize the value taken in UCA1RXBUF to control the duty cycle of the LED.


