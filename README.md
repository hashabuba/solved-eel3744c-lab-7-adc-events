Download Link: https://assignmentchef.com/product/solved-eel3744c-lab-7-adc-events
<br>
<h1>OBJECTIVES</h1>

<ul>

 <li>Develop a basic understanding of converting signals from the analog continuous-time domain to the digital discrete-time domain using the ADC system within the <em>ATxmega128A1U</em>.</li>

 <li>Use and understand the purpose of the event system within the <em>ATxmega128A1U</em></li>

 <li>Create a basic oscilloscope program by using the ADC and event systems within the XMEGA, as well as the <em>SerialPlot</em> program</li>

</ul>

<h1>INTRODUCTION</h1>

As you have seen throughout the previous labs, your microcontroller can produce and detect discrete binary values, i.e., ‘0’ corresponding to a voltage value close to your processor’s ground reference (normally 0 V), and ‘1’ corresponding to a voltage value close to your processor’s V<sub>CC</sub> reference (e.g., 3.3 V). However, the world is also filled with analog signals, i.e., signals that have more than two discrete values, such as those that represent velocity, temperature, sound and light intensities, etc. Thankfully, it is possible for your microcontroller to both interpret and generate these non-digital signals, through two separate systems: An <strong>A</strong>nalog-to-<strong>D</strong>igital <strong>C</strong>onverter (<strong>ADC</strong>), and a <strong>D</strong>igital-to-<strong>A</strong>nalog <strong>C</strong>onverter (<strong>DAC</strong>).

<strong>NOTE: </strong>Many sensors output analog signals; an ADC is often used to convert their outputs to a digital representation. Some sensors perform this conversion themselves.

With an ADC system, an analog signal can be sampled and converted to a digital value. This digital value, whose limits are determined by the hardware and software configurations of the ADC system, can then be interpreted by your microcontroller.

<h1>LAB STRUCTURE</h1>

In this lab, we will leverage the ADC system within the XMEGA to ultimately create a program that functions like a simple oscilloscope. An analog input will be displayed on your computer screen via<em> SerialPlot</em>.




<table width="687">

 <tbody>

  <tr>

   <td width="389"><strong><u>REQUIRED MATERIALS</u></strong>•       <a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8331_%20XMEGA_AU_Manual.pdf">Atmel </a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8331_%20XMEGA_AU_Manual.pdf"><em>ATxmega128A1U</em></a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8331_%20XMEGA_AU_Manual.pdf"> AU Manual (doc8331)</a>•       <a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8385_ATxmega128A1U_Manual.pdf">Atmel </a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8385_ATxmega128A1U_Manual.pdf"><em>ATxmega128A1U</em></a><a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8385_ATxmega128A1U_Manual.pdf"> Manual (doc8385)</a>•       <em>OOTB µPAD</em>, with USB A/B cable•       <em>OOTB Analog Backpack</em>,with accompanying schematic•       <strong>D</strong>igital <strong>A</strong>nalog <strong>D</strong>iscovery (<strong>DAD</strong>) kit, with <em>WaveForms</em></td>

   <td width="299"><strong><u>SUPPLEMENTAL MATERIALS</u></strong>•       <a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8032_ADC.pdf">AVR1300: Using the AVR XMEGA ADC</a>•       <a href="https://mil.ufl.edu/3744/docs/XMEGA/doc8071_events.pdf">AVR1001: Getting Started with the Event System</a>•       <a href="https://bitbucket.org/hyOzd/serialplot/downloads/"><em>SerialPlot</em></a> source website o     <a href="https://bitbucket.org/hyOzd/serialplot/downloads/serialplot-0.10.0-win64.exe"><em>SerialPlot</em></a> for 64-bit Windows o          <a href="https://bitbucket.org/hyOzd/serialplot/downloads/serialplot-0.10.0-win32.exe"><em>SerialPlot</em></a> for 32-bit Windows</td>

  </tr>

 </tbody>

</table>




<strong> </strong>

<h1>PRE-LAB PROCEDURE</h1>

<strong><u>REMINDER OF LAB POLICY</u></strong>

As required, you must re-read the <a href="https://mil.ufl.edu/3744/admin/Lab%20Rules%20%26%20Policies.pdf"><em>Lab Rules &amp; Policies</em></a> before submitting any pre-lab assignment and before attending any lab.

<h2>1. USING THE ADC SYSTEM</h2>




In this section, you will write a program, <strong>lab7_1.c</strong>, to sample data from the CdS cell located on the <em>OOTB Analog Backpack</em>.

<strong>NOTE:</strong> A Cadmium-Sulfide (<em>CdS</em>) photocell (also known as a photoresistor, or a light-dependent resistor [<em>LDR</em>]) is a resistor whose resistance is variable to the amount of light present on the surface of the photo-conductive cell (see Figure 1). The CDS cell located on your <em>OOTB Analog Backpack</em> is labeled <em>CDS1</em>.

<strong> </strong>

<strong>Figure 1: </strong>Two CdS photocells

However, before interfacing with the CdS photoresistor cell, you must familiarize yourself with the ADC system within the <em>ATxmega128A1U</em>.

1.1. Read the <em>AVR1300</em> Application Note describing the ADC system within XMEGA microcontrollers. Re-enforce the concepts from this document by reading § 28 (<em>ADC – Analog-to-Digital Converter</em>) of the doc8331 manual.

<h3>PRE-LAB EXERCISES</h3>

<ol>

 <li>Why must we use the ADCA module as opposed to the ADCB module?</li>

</ol>

As alluded to above, an ADC system within your microcontroller will be used to sample analog voltage values generated by the CdS cell located on the <em>OOTB Analog Backpack</em>. Below, you will write software to interface with the CdS cell, although it will first be necessary to identify which ADC system must be used within the XMEGA.

1.2. Determine which ADC module must be used to interface with the CdS cell located on the <em>OOTB Analog Backpack</em>. Refer to the <em>OOTB Analog Backpack Schematic</em>.

1.3. Write a “C” function, <strong><em>void adc_init(void)</em></strong>, to initialize the ADCA module as follows:

<ul>

 <li>12-bit signed, right-adjusted</li>

 <li>Normal, i.e., not <em>freerun</em> mode</li>

 <li>5 V voltage reference</li>

</ul>

Only enable the module after all ADC initializations have been made, and do <em>not</em> start a conversion within the initialization function.

In the <em>MUXCTRL</em> register, select the appropriate combination of positive and negative inputs to measure the voltage at the CdS cell. The CDS+ and CDS- signals on the <em>OOTB Analog Backpack</em> schematic should be used.

<h3>PRE-LAB EXERCISES</h3>

<ol>

 <li>Would it be possible to use any other ADC configurations such as single-ended, differential with gain, etc. with the current pinout of the <em>OOTB Analog Backpack</em>? Why or why not?</li>

</ol>

<ul>

 <li>What would the main benefit be for using an ADC system with 12-bit resolution, rather than an ADC system with 8-bit resolution? Would there be any reason to use 8-bit resolution instead of 12-bit resolution? If so, explain.</li>

</ul>

1.4. Write a main routine for your program that <em>continually </em>does the following:

<ol>

 <li>Start an ADC conversion on the proper ADC channel.</li>

 <li>Wait for the proper ADC interrupt flag to be set, indicating that the conversion has finished.</li>

</ol>

<ul>

 <li>Store the 12-bit signed conversion result into a signed 16-bit variable.</li>

</ul>

1.5. Verify that some ADC conversion results are accurate. This can easily be achieved by placing a breakpoint after you save the conversion result and viewing its contents in the <em>Watch</em> window.

<table width="735">

 <tbody>

  <tr>

   <td width="733"><strong>2.</strong> <strong>SAMPLING AT A SPECIFIC RATE USING EVENTS</strong>In this section, you will write a program, <strong>lab7_2.c</strong>, that will 2.1. Read § 6 (<em>Event System</em>) of the doc8331 manual as well as sample the CdS cell three times per second. A timer will be used the <em>AVR1001</em> application note, which is provided in the to <em>automatically</em> trigger ADC conversions at a frequency of <em>Supplemental Materials</em> section of this document.3 Hz. For this to happen automatically, you <em>must</em> use the EventWhen reading about events, you do not need to pay attention toSystem.  any information regarding the quadrature decoder. It is not relevant for our course.</td>

  </tr>

 </tbody>

</table>

To check if the result is correct, find an appropriate equation of a line that represents the linear relationship between the range of measurable analog voltage values and the range of digital values of the ADC system (e.g., <strong>V<sub>ANALOG</sub> = f(V<sub>DIGITAL</sub>)</strong>). For example, with an analog voltage range of −2.5 V to +2.5 V, if the analog voltage is 1V, the digital representation should be about 51<sub>10</sub> = 0x33 for an 8-bit ADC system, and 819<sub>10</sub> = 0x333 for a 12-bit ADC system; if the voltage is 1.5V, the digital representation should be about 77<sub>10</sub> = 0x4D for an 8-bit ADC system, and 1228<sub>10</sub> = 0x4CC for a 12-bit ADC system. You can measure the voltage difference across the CDS+ and CDS- pins using your multimeter and then compare that to the voltage value you calculate from your ADC result.

2.2. Write a “C” function, <strong><em>void</em></strong> <strong><em>tcc0_init(void)</em></strong>, to initialize the TCC0 timer/counter module to overflow three times a second.

<ul>

 <li>Use any valid prescaler/period combination to achieve an overflow time of one-third of a second.</li>

 <li>Use the TCC0 overflow to trigger an event on <em>Event Channel 0</em>. This can be done using the <em>CH0MUX</em> register within the Event System.</li>

</ul>

2.3. Make the following additions to the <strong><em>adc_init</em></strong> function you

Ø Using the <em>EVCTRL</em> register within the ADC module, make an ADC conversion start when <em>Event Channel 0</em> is triggered.

2.4. Write the ADC interrupt service routine that is executed when a conversion is complete. It should do the following:

<table width="735">

 <tbody>

  <tr>

   <td width="709">           wrote in § 0:                                                                                                    <strong>NOTE:</strong> The rate at which the LED toggles should be identicalto your sampling rate, i.e., 3 Hz. Ø Enable an ADC interrupt to be triggered when aconversion is complete.<strong>3.</strong> <strong>OUTPUTTING SAMPLED DATA WITH UART</strong></td>

  </tr>

 </tbody>

</table>

<ol>

 <li>Save the result into a signed 16-bit integer variable, just like you did in § iii. ii. Toggle the <em>GREEN_PWM</em> LED located on the <em>µPAD</em>.</li>

</ol>




In this section, you will test the functionality of your current system by outputting the <em>analog voltage value</em> measured on the CdS cell every second, and then display the results, in terms of both decimal and hexadecimal, within a serial terminal program on your computer (e.g., PuTTY). More specifically, <em>the output displayed within your terminal program must include at least the following to describe the voltage measured:</em>

(<strong><em>+/-)voltage_decimal V (0xvoltage_hex)</em></strong> where <strong><em>voltage_decimal</em></strong> is the measured ADC digital value corresponding to the voltage drop experienced by the CdS photocell, in terms of a decimal value with two decimal places, and where <strong><em>voltage_hex</em></strong> is the measured ADC digital value corresponding to the voltage drop experienced by the CdS photocell, in terms of a hexadecimal value with three digits.

For example, if an 8-bit ADC system with a voltage range of  -5 V to +5 V was used, and the measured voltage was to correspond to a decimal value of 1.37 V, your output should include “<strong>+1.37 V (0x22)</strong>”, without the quotes.

Moreover, if for the same system, a measured voltage value was to correspond to a decimal value of -2.52 V, your output should include “<strong>-2.52 V (0xC0)</strong>”, also without the quotes.

<h3>PRE-LAB EXERCISES</h3>

<ol>

 <li>What is the decimal voltage value that is equivalent to a 12-bit signed result of 0x073?</li>

</ol>

3.1. Using everything from the previous two sections, create a voltmeter program, <strong>lab7_3.c</strong>, that outputs the voltage at the CdS cell to a serial terminal program every second.

3.2. In your ADC ISR, update a global variable with the new ADC conversion result, and set a global flag that indicates a new conversion has been made.

3.3. Create a “C” function, <strong><em>void usartd0_init(void)</em></strong>, that initializes the USARTD0 module to operate at 115,200 bps, with eight data bits, no parity, and one stop bit.

3.4. In your main routine, when the flag gets set, do the following:

3.4.1    Clear the global flag.

3.4.2    Output the voltage to the serial terminal.

First send the sign, either ‘+’ or ‘−’ out of the XMEGA’s serial port, i.e., transmit it to your computer.

The below algorithm describes how you could output the digits of a decimal number.  (Remember that, for this course, you are <em>not</em> <em>allowed</em> to use standard “C” functions like sprint or printf.)

<ul>

 <li>Pi = 3.14159…//variable holds original value</li>

 <li>Int1 = (int) Pi = 3 è 3 is the first digit of Pi</li>

 <li>Transmit Int1 and then “.”</li>

 <li>Pi2 = 10*(Pi – Int1) = 1.4159…</li>

 <li>Int2 = (int) Pi2 = 1 è 1 is the second digit of Pi</li>

 <li>Transmit the Int2 digit</li>

 <li>Pi3 = 10*(Pi2 – Int2) = 4.159…</li>

 <li>Int3 = (int) Pi3 = 4 è 4 is the third digit of Pi</li>

 <li>Transmit the Int3 digit, then a space, and then a ‘V’ Transmitted Result: <strong>14 V</strong></li>

</ul>

Make sure that after every voltage transmission, you send a carriage return and line feed characters such that the next voltage is displayed on a new line.




<h2>4. VISUALIZING THE ADC CONVERSIONS</h2>

In this section, you will write a program, <strong>lab7_4.c</strong>, to visually 4.2. In your main routine, instead of outputting the decimal display the CdS cell’s voltage using <em>SerialPlot</em>, like you did in voltage value as done in § 3, you will output the raw 16-bit Lab 5 with the IMU’s accelerometer. This will behave like a very signed values via UART using <em>Simple Binary</em> data format basic oscilloscope.  that <em>SerialPlot</em> understands. Now, since you are only

outputting one type of data (ADC channel conversion), you

4.1. Modify your <strong><em>tcc0_init</em></strong> function to make the timer overflow only need to use one channel in <em>SerialPlot</em>. This means that

at 100 Hz instead of 3 Hz. You may need to make changes you only need to output two bytes via UART every time

to your prescaler.

you get a new conversion result. Don’t forget to update

your <em>SerialPlot</em> configurations accordingly. Leave the You should be able to see the waveform in a the <em>SerialPlot</em> variable type the same (<em>int16</em>) and change the number of window, like when it was used with the accelerometer.  channels to one.

<h2>5. SWITCHING BETWEEN MULTIPLE INPUTS</h2>




Finally, you will write a program, <strong>lab7_5.c</strong>, to use UART to switch between two different analog inputs: the CdS cell and the analog input jumper labeled <em>J3</em> on the <em>OOTB Analog Backpack</em>.

5.1. Modify your previous <strong><em>usartd0_init</em></strong> function to enable interrupts for the UART receiver.

<strong>NOTE: </strong>To send data from <em>SerialPlot</em> to your microcontroller, you will use the <em>Commands</em> tab within SerialPlot. Enter the character you wish to send in the field next to “Command 1” and then click <em>Send</em>. You can create and save multiple unique commands if desired.<strong>  </strong>

When the character ‘C’ is received via your serial terminal connection, the program should switch to <em>continually </em>measuring and outputting the CdS cell data to <em>SerialPlot</em> at a rate of 100 Hz, where an error of up to 2% is permitted for the generated frequency.

<h2><u>PRE-LAB PROCEDURE SUMMARY</u></h2>

<ol>

 <li>Write lab7_1.c to ensure your ADC initializations are correct.</li>

</ol>

When the character ‘J’ is received, the program should switch to measuring the result of the analog input jumper <em>J3</em>, located on the <em>OOTB Analog Backpack</em>.

If any other characters are received, don’t change anything.

The decision to switch input sources should occur only within the receiver interrupt service routine.

<strong>NOTE:</strong> You will most likely be using the function generator of your DAD board to supply the ADC with an analog signal via the <em>J3</em> header.  You should, however, be able to visualize waveforms from other entities, such as an external function generator or circuit.

Be aware that this will not work over all input frequencies. You may want to explore the correlation between the frequency of an input signal and the affect it has on your program’s output.




<ol start="2">

 <li>Write lab7_2.c to introduce the concept of sampling at specific intervals.</li>

 <li>Write lab7_3.c to create a voltmeter-like application with UART.</li>

 <li>Write lab7_4.c to create a single-input basic oscilloscope.</li>

 <li>Write lab7_5.c to expand on the topics from § 4.</li>

</ol>




<strong> </strong>




<h1>APPENDIX</h1>

<h2>A. TROUBLESHOOTING SERIAL PLOT</h2>

The goal of this section is to help you with some of the issues students typically have with the serial plot software.

<strong><u>Problem #1 (Flipped Low &amp; High Bytes)</u> </strong>

<ul>

 <li></li>

</ul>

<table>

 <tbody>

  <tr>

   <td width="41"></td>

  </tr>

  <tr>

   <td></td>

   <td></td>

  </tr>

 </tbody>

</table>

<ul>

 <li>Many students run into an issue where their data seems to fluctuate rapidly through a large range of values. This typically happens when the data from your <em>µPAD</em> is flipped. <em>SerialPlot</em> is taking in your low byte as a high byte and your high byte as the low byte. The simplest way to fix this is to restart you program and ensure that Serial Plot is running before you start the program on your <em>µPAD</em>.</li>

</ul>

<strong><u>Problem #2 (Baud Rate)</u> </strong>

<ul>

 <li>If you are getting junk data from your <em>µPAD</em> on <em>SerialPlot</em>, this usually indicates that the Baud Rate is incorrectly input into <em>SerialPlot</em>. Check both your code and <em>SerialPlot</em> values to ensure that they match.</li>

</ul>

<strong><u>Problem #3 (PuTTY)</u> </strong>

<ul>

 <li>If you are getting no data from your <em>µPAD</em>, ensure that you do not have PuTTY running. Only one program can have control over a serial connection of your <em>µPAD</em>, and PuTTY and <em>SerialPlot</em> don’t get along, so only use one at a time.</li>

</ul>