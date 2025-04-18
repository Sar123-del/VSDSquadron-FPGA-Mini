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

# Task 1 
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


# Task - 2 
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

 ![Image](https://github.com/user-attachments/assets/dfac0484-744e-4d7f-9321-d1673d234467)

   **Step -3 Implementation**

   Makefile and verilog file given to us can be access from here - https://github.com/thesourcerer8/VSDSquadron_FM/tree/main/uart_loopback 
   
   1. Open terminal
   2. Change the directory to which your verilog file and pcf are there.
      enter - cd Uart
   3. Enter the makefile
      
      ![Untitled](https://github.com/user-attachments/assets/d097944d-9d1b-4e86-95eb-858202ddb309)
      
   4. Now a red led will glow and our code has been run successfully
      
      ![Untitled](https://github.com/user-attachments/assets/4279862f-7ef3-493c-bc5d-1ebdfe34c563)

  **Step -4 Testing and Verification**

 1. For this we will be using a software known as docklight, we can download it from their own website
 2. Open docklight, check if your system is connected to the right COM. In my case it is COM7. Verify that the speed is set to 9600
    
  ![Screenshot (154)](https://github.com/user-attachments/assets/d57666a1-daa4-4872-be38-662af0595f18)

 3. Then, double click on the small blue box below Send Sequences and enter the name and sequence and click ok.
    
    ![Screenshot (157)](https://github.com/user-attachments/assets/5b87173c-4ce8-4d78-b03a-9ccdf091ebbf)

 4. Click an arrow below Tools and then click the arrow near the name in the blue box and verify the results

    ![Screenshot (158)](https://github.com/user-attachments/assets/fccda6ab-9ab5-494b-9a1f-2b3b96901478)

    **Video demonstration**

 https://github.com/user-attachments/assets/370e20cc-b9b6-417b-b359-ec561e7105f0

 # Task -3
 
 **Step - 1  Study the Existing Code**

 **Overview**
 
 This project focuses on FPGA-based digital design. The UART transmitter (uart_tx) module is responsible for serial communication by converting parallel data into a serial data stream.   This is crucial for interfacing FPGA boards with external devices such as PCs or microcontrollers.

 **Verilog Code Analyze**

1.  *top.v (Top-Level Module)*

    Acts as the main module, integrating different components.

    Instantiates the UART transceiver (uart_trx.v) along with other logic.

    Manages clocking, reset signals, and data flow.

    Defines I/O connections for FPGA hardware interfacing.

2. *uart_trx.v (UART Transceiver)*

    Implements both transmitter (TX) and receiver (RX) functionalities.

    Handles data framing (start bit, 8 data bits, optional parity, stop bit).

    Manages baud rate generation to ensure correct timing.

    Uses a Finite State Machine (FSM) to control transmission and reception.
   
   **Pin Mapping**
   
   The PCF (Physical Constraints File) in the VSDSquadron_FM UART project is used to map logical signals from the Verilog code to specific FPGA pins. It ensures that important signals      like the system clock, reset, UART transmit (TX), and UART receive (RX) are correctly connected to the FPGA hardware.

   For example, the TX signal is assigned to a particular FPGA pin to send serial data, while the RX signal is mapped to receive incoming data. The clock and reset signals are also         assigned to appropriate pins to control the system timing and initialization.

   To ensure proper operation, the PCF file should match the specific pin layout of the FPGA board. If needed, the pin mappings can be modified based on the board’s documentation.

   **Step - 2 Visual Description**

   A block diagram detailing the UART transmitter module.

 ![uart_rx_tx](https://github.com/user-attachments/assets/81ef0d26-2dcb-48e2-aeb8-027f69bc3256)

 A circuit diagram illustrating the FPGA's UART TX pin connection to the receiving device.

 ![circuit 3 ](https://github.com/user-attachments/assets/5d3ffcea-3d5c-4602-a58e-1d4bb9a330e7)

 **Step - 3  Implementation**

 The files given to us are - https://github.com/thesourcerer8/VSDSquadron_FM/tree/main/uart_tx
 
 1. Open terminal
 2. Change the directory to the folder in which you have all the files (verilog code, PCF file, makefile)
    
    ![Untitled](https://github.com/user-attachments/assets/456218c4-2743-48c1-8563-e5b8bd52dff5)

 3. Enter the makefile
    ![Untitled](https://github.com/user-attachments/assets/739e8f05-df48-4664-9720-22f32a597c3c)

 4. Rgb led will start glowing

 That's all our  code is transmitted
 
 **Step - 4  Testing and Verification**
 1. Install the picocom by entering - sudo apt install picocom
 2. Enter - make terminal
 3. A series of D will start coming in the terminal
    ![Untitled](https://github.com/user-attachments/assets/10357aeb-fe31-451c-b3fe-5bc1ed81edbf)

  **Video demonstartion**

  https://github.com/user-attachments/assets/d6591281-ba36-429b-bc70-c8d015d2ab9e
  
   # Task -3
 
 **Step - 1  Study the Existing Code**

  The uart_tx_sense module in the VSDSquadron_FM repository is designed to acquire sensor data and transmit it via UART. Here's an overview of its functionality:​

   1. Clock Management:

      The module utilizes an internal oscillator (SB_HFOSC) to generate a stable clock signal.​
       
      A clock divider is implemented to derive the baud rate necessary for UART communication. This divider ensures that data transmission aligns with standard UART timing requirements.​

   2. State Machine for UART Transmission:

      The module employs a finite state machine (FSM) to manage the UART transmission process. The FSM transitions through states such as IDLE, START, DATA, and STOP to control the            sequence of data transmission.​
       
      In the IDLE state, the transmitter awaits a load signal indicating that data is ready for transmission. Upon receiving this signal, it moves to the START state to send the               start bit, followed by the DATA state to transmit the actual data bits, and finally to the STOP state to send the stop bit before returning to IDLE.​
        

   3. Sensor Data Acquisition:

       The module interfaces with sensors to collect data, which is then prepared for transmission. The specifics of sensor interfacing depend on the sensor type and communication              protocol used.​

   4. Data Transmission:

      Once sensor data is acquired, it's loaded into a shift register. The FSM controls the shifting process to serially transmit the data bits over the UART interface.​
      
      The module ensures that each byte of data is framed with start and stop bits, adhering to the standard UART protocol.

      **Pin Mapping**
     
        Clock Input (clk)

         An internal oscillator (SB_HFOSC) generates the clock signal used for UART transmission timing.

        UART Transmit (tx pin)

         The UART transmission pin (tx) sends sensor data serially. It is driven by the state machine handling UART transmission.

       Sensor Data Input (sensor_data or equivalent)

         The module reads sensor data, which is then transmitted over UART. The specific pin depends on the sensor connection.

       Load/Enable Signal (load or tx_en)

         This signal triggers data transmission when new sensor data is ready.

   **Step - 2 Visual Description**

  A block diagram depicting the integration of the sensor module with the UART transmitter.
  
 ![Image](https://github.com/user-attachments/assets/fddd5f7a-d48d-485b-94ba-0c0bef53d02d)

  A circuit diagram showing connections between the FPGA, sensor, and the receiving device.
  
 ![52a01124-9e9a-45d6-900a-2b1942e94696](https://github.com/user-attachments/assets/946b7f27-89cf-4234-b175-5d489b2f3485)


 **Step - 3  Implementation**

 The files given to us are - https://github.com/thesourcerer8/VSDSquadron_FM/tree/main/uart_tx_sense
 
 1. Open terminal
 2. Change the directory to the folder in which you have all the files (verilog code, PCF file, makefile)
    
   ![Untitled-1](https://github.com/user-attachments/assets/a5f3764c-cff6-419f-9201-fb2715ddca8f)

 3. Enter sudo make build and sudo make flash
    
   ![Untitled-1](https://github.com/user-attachments/assets/0d1316f0-2075-4070-b157-b537f617fb45)

   ![Untitled-1](https://github.com/user-attachments/assets/a3d29c0e-6f9b-4ca4-ac45-6de646cfcce5)

 4. Red led will start glowing

 That's all our  code is transmitted

  **Step - 4  Testing and Verification**
 1. Install the picocom by entering - sudo apt install picocom
 2. Enter - make terminal
 3. A series of D will start coming in the terminal
    
 ![Untitled-1](https://github.com/user-attachments/assets/061ec59f-81e0-4a6b-b88e-ea7d004d9a3c)

![Untitled-1](https://github.com/user-attachments/assets/7ff45c76-6d16-4dbd-b8eb-eeb93eea2c3a)

**Video demonstration**

https://github.com/user-attachments/assets/33e8fb6e-daad-487e-8302-82dd0b5673c8

# Task - 5 and 6

**Real-Time Sensor Data Acquisition and Transmission System**

**Overview**
To design and implement a real-time system that acquires distance measurements from an ultrasonic sensor (HC-SR04), processes the data using an FPGA (iCE40HX1K on VSDSquadron board), and prepares it for transmission or further interfacing.

 **System Description**

This project involves interfacing the HC-SR04 ultrasonic sensor with a  FPGA board. The system sends a trigger signal to the sensor, receives the echo pulse indicating distance, and uses the FPGA’s internal logic to measure the time-of-flight. This timing data represents the distance to an object and can be used for a variety of embedded and real-time applications.

   **Key Features**
    Real-time processing using Verilog on the iCE40HX1K FPGA.Trigger generation and echo pulse measurement logic.Precise distance timing based on the echo pulse width.Configurable via constraints (.pcf) and fully open-source toolchain (Yosys, nextpnr, IceStorm).Ready for UART/Display interfacing (optional extension).

 **Working Principle**
    The FPGA sends a 10μs HIGH pulse to the TRIG pin of the HC-SR04.The sensor emits an ultrasonic burst and waits for it to reflect. When the echo is detected, the ECHO pin goes HIGH       for a duration proportional to the distance.The FPGA measures this HIGH duration using a counter running on the system clock.The result can be displayed or transmitted via UART 

  **Project Files**

   top.v: Verilog RTL for pulse generation and echo timing. Constraints.pcf: FPGA pin mapping specific to the VSDSquadron board.  Makefile: Automates synthesis, place-and-route, and          bitstream generation.top.bin: Final output bitstream for flashing onto the FPGA.

 **Outcome**

This project successfully demonstrates a low-cost, FPGA-based real-time sensing system using basic Verilog and open-source tools. It serves as a foundational block for more complex applications such as robotics, distance measurement systems, and real-time embedded control.

 **Visual Description**

  Block Diagram
  ![Image](https://github.com/user-attachments/assets/976b4c2a-a539-4b27-a31f-554989eeeca3)
  
  Circuit Diagram
 ![b0dd9f2e-61f6-40b6-be9d-f3c0b04111fc](https://github.com/user-attachments/assets/58f956a2-058e-4e8e-a2ac-cddb2300a32a)

 **Implementation**
 1. Open the terminal
 2. Change the directory to the folder in which you have your verilog file, pcf file, makefile
 3. Enter sudo make build and sudo make flash
    That's all program has been uploded

 **Testing and Verfication**
 1. Install picocom by entering sudo apt install picocom
 2. Enter make terminal
 3. Serial teminal will open and distance will come

 **Video Demostartion**

 https://github.com/user-attachments/assets/cae671fa-d5d9-4a52-a158-7cc39ce7dd3a
 





 


    








   
   

   


 

 






