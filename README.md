# VSDSquadron-FPGA-Mini
![images](https://github.com/user-attachments/assets/a6e22a90-a895-4119-9775-e3375795449f)

**VSDSquadron FPGA Mini Board**
The VSDSquadron FPGA Mini (FM) board is a compact and affordable development platform designed for FPGA prototyping and embedded system applications. It features the Lattice iCE40UP5K FPGA, providing a balance between flexibility and power efficiency.

**Key Features:**

FPGA: Lattice iCE40UP5K (5.3K LUTs, 1Mb SPRAM, 120Kb DPRAM)

Connectivity: FTDI FT232H USB-to-SPI for programming and communication

GPIO: 32 FPGA GPIOs exposed for interfacing and prototyping

Memory: 4MB SPI Flash for configuration and data storage

Power: Onboard 3.3V and 1.2V regulators, supports external 3.3V supply

LEDs: Integrated RGB LED for status indication or custom applications

Form Factor: 57mm × 29mm, compact with accessible pins

**Applications:**

FPGA-based hardware prototyping

Embedded control systems

Digital logic design experiments

Custom LED drivers and visual indicators

FPGA education and learning

## Task 1 ##
**Step 1 - Understanding verilog code**
The module given to us is this - https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/led_blue/top.v

**Purpose of the Module**

The  module is designed to implement an LED control system using an internal  oscillator. It generates a frequency counter and drives an RGB LED through an FPGA's built-in driver primitive. The purpose is to create a simple LED control mechanism using internal logic.

**Description of Internal Logic and Oscillator**

The module instantiates an internal high-frequency oscillator (SB_HFOSC) to generate a clock signal.The int_osc signal serves as the clock input for a 28-bit counter (frequency_counter_i).The counter continuously increments on every rising edge of int_osc.A test output signal (testwire) is derived from bit 5 of the counter to allow external observation of the internal frequency behavior.

**Functionality of the RGB LED Driver**

The module instantiates an FPGA-specific RGB LED driver primitive (SB_RGBA_DRV), which controls the red, green, and blue LED outputs.The driver enables current flow to the RGB LED components through three PWM signals:RGB0PWM (Red) is set to 0 (OFF).RGB1PWM (Green) is set to 0 (OFF).RGB2PWM (Blue) is set to 1 (ON), which keeps the blue LED active.The CURREN signal ensures the current source for the LEDs is enabled.The RGBLEDEN signal enables the RGB LED driver functionality.Current settings for each LED color are defined using defparam, with a intensity (0b000001).

**Relationship to Outputs**

led_red, led_green, and led_blue are the actual hardware connections that drive the RGB LED.The module ensures that only the blue LED remains ON while red and green remain OFF.The testwire output allows monitoring of the frequency counter operation by exposing its 5th bit.This module is useful for basic LED control applications using an FPGA

**Step 2 - Creating the PCF File**
The PCF file given to us - https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/led_blue/VSDSquadronFM.pcf

The pin assignment table given in the datasheet -

![Screenshot (150)](https://github.com/user-attachments/assets/bc7638b8-8f1b-431b-9458-2ccbaaaea18e)

**Significance of Each Connection**


led_red	39
led_blue 40
led_green 41
hw_clk 20
testwire 17


led_red, led_blue, led_green (Pins 39, 40, 41): These pins are assigned to drive the RGB LED on the FPGA board. The module ensures that the blue LED is ON while the red and green LEDs are OFF.


hw_clk (Pin 20): This pin is used as the external hardware oscillator input. It is not utilized in the current module, but could be used for precise timing instead of the internal oscillator.


testwire (Pin 17): This pin outputs bit 5 of the frequency counter, providing an external indication of the internal counter state.

**Step 3 - Integrating with the VSDSquadron FPGA Mini Board**
This was the makefile given to us - https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/led_blue/Makefile
Following are the steps to integrate the file with VSDSquadron FPGA board -
1. Open the terminal
2. Change the directory to the folder in which you have the verilog code and pcf file
   Write - cd blue_led
   
   ![Untitled](https://github.com/user-attachments/assets/8908ae68-8435-49ad-84cc-e07549b6daa1)

   
3. Now enter the Makefile

   
   ![Untitled](https://github.com/user-attachments/assets/2d65c55a-fd45-4e18-85a8-ea36ec4b01ba)

   
4. Now the programming will start and blue led will be flashed in the board


   
   ![Untitled](https://github.com/user-attachments/assets/7cd2cfbd-0d6a-47c2-b0bd-dd3013561e0b)

   
   ![Untitled](https://github.com/user-attachments/assets/53e31433-c120-4a10-918c-410110cfd732)

Our program has been run successfully


## Task - 2 ##
**Step -1 Study the Existing Code**

The files given to us can be accessed from here- https://github.com/thesourcerer8/VSDSquadron_FM/tree/main/uart_loopback

Overview

The uart_loopback project in the VSDSquadron_FM repository implements a UART (Universal Asynchronous Receiver-Transmitter) loopback system. The loopback mechanism sends received data back to the transmitter without modification.

This functionality is implemented in two main Verilog files:

*top.v* - The top-level module integrating UART communication, clock management, and RGB LED status indicators.

 *uart_trx.v* - The UART transmitter and receiver module that handles serial communication.

**Understanding Loopback in top.v**

1. Module Declaration

   The top module defines inputs and outputs for UART and LED indicators. The main signals involved are - the system clock, active-low reset, UART receive and transmit pins, and an RGB      LED output for status indication.

2. Internal Oscillator and Clock Management

    An internal oscillator generates a stable clock signal used for UART operations. A frequency divider reduces the high-frequency clock to a lower frequency suitable for serial             communication.

3. UART Loopback Logic

    The core loopback functionality is implemented using an instance of the UART transmitter and receiver module. The received data is directly assigned as the transmit data, ensuring        that any data received is immediately sent back. Transmission is triggered whenever new data is received, completing the loopback process.

4. Visual Feedback with RGB LED

   The first three bits of the received UART data control an RGB LED. This provides a visual indication of incoming data, helping to verify UART activity.

**Understanding uart_trx.v (UART Transmission & Reception)**

1. UART Transmitter

   The transmitter takes parallel data and converts it into a serial data stream. It includes a start bit, 8-bit data, and a stop bit. A shift register is used to send one bit at a time.

2. UART Receiver

   The receiver detects the start bit and shifts in the incoming serial data. Once all bits are received, the data is stored and marked as available for processing. This allows the          received data to be looped back to the transmitter.

   **Conclusion**
  
   The loopback mechanism ensures that any data sent to the FPGA via UART is echoed back to the sender. Additionally, the first three bits of received data are displayed on an RGB LED       for visual confirmation.

  **Step -2 Visual Desription**
  
   A block diagram illustrating the UART loopback architecture
   
   ![Image](https://github.com/user-attachments/assets/7bf3f3f8-b1bf-4c3d-a794-7cf7822c7e11)

   The key components of the block diagram are -
   
1. Clock – Generates a system clock signal for synchronization.
  
2. UART Receiver – Receives serial data (RX) and converts it into parallel format.

3. Loopback Block – Directly connects the received data to the transmitter, ensuring the same data is sent back.

4. UART Transmitter – Converts parallel data back into serial format and transmits it (TX).

5. RGB LED – Displays the first three bits of the received data as a visual indication of UART activity.

6. Arrows – Indicate the direction of data flow from RX to Receiver, through the Loopback Block, to the Transmitter, and out as TX. The LED is connected to the Receiver for visual feedback.

 A detailed circuit diagram showing connections between the FPGA and peripheral devices used.
 ![Image](https://github.com/user-attachments/assets/3af0100e-e1d4-4b37-8a2f-7ec84fed7847)
   
   

   


 

 






