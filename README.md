# VSDSquadron-FPGA-Mini
## Task 1 ##
**Step 1 - Understanding verilog code**
The module given to us is this - https://github.com/thesourcerer8/VSDSquadron_FM/blob/main/led_blue/top.v

**Purpose of the Module**

The  module is designed to implement an LED control system using an internal  oscillator. It generates a frequency counter and drives an RGB LED through an FPGA's built-in driver primitive. The purpose is to create a simple LED control mechanism using internal logic.

**Description of Internal Logic and Oscillator**

The module instantiates an internal high-frequency oscillator (SB_HFOSC) to generate a clock signal.

The int_osc signal serves as the clock input for a 28-bit counter (frequency_counter_i).

The counter continuously increments on every rising edge of int_osc.

A test output signal (testwire) is derived from bit 5 of the counter to allow external observation of the internal frequency behavior.

**Functionality of the RGB LED Driver**

The module instantiates an FPGA-specific RGB LED driver primitive (SB_RGBA_DRV), which controls the red, green, and blue LED outputs.

The driver enables current flow to the RGB LED components through three PWM signals:

RGB0PWM (Red) is set to 0 (OFF).

RGB1PWM (Green) is set to 0 (OFF).

RGB2PWM (Blue) is set to 1 (ON), which keeps the blue LED active.

The CURREN signal ensures the current source for the LEDs is enabled.

The RGBLEDEN signal enables the RGB LED driver functionality.

Current settings for each LED color are defined using defparam, with a intensity (0b000001).

**Relationship to Outputs**

led_red, led_green, and led_blue are the actual hardware connections that drive the RGB LED.

The module ensures that only the blue LED remains ON while red and green remain OFF.

The testwire output allows monitoring of the frequency counter operation by exposing its 5th bit.

This module is useful for basic LED control applications using an FPGA 


