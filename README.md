# Motor Control using FPGA and PWM

This project demonstrates how to use an FPGA to control a DC motor using Pulse Width Modulation (PWM). The FPGA used for this project is the Xilinx Spartan-6, specifically the XC6SLX16-2FTG256C.

## Table of Contents
1. [Introduction](#introduction)
2. [Why FPGA for Motor Control](#why-fpga-for-motor-control)
3. [What is PWM](#what-is-pwm)
4. [Hardware Required](#hardware-required)
5. [Setup and Implementation](#setup-and-implementation)
6. [VHDL Code](#vhdl-code)
7. [Demo](#demo)
8. [References](#references)

## Introduction

The primary goal of this project is to create a motor control system using FPGA and PWM. The system will allow the user to control the speed of a DC motor using push buttons for incrementing and decrementing the speed.

![Spartan 6 FPGA Board](path_to_image/FPGA_board_image.jpg)

## Why FPGA for Motor Control

- **Lower System Cost**
- **Concurrency in the Algorithm (Parallel Processing)**
- **Sufficient I/Os**
- **Design Reuse and Scalability**
- **Extended Product Lifecycles**
- **Control Multiple DC Motors Synchronously**
- **Drive a Stepper Motor with an Analog Feedback System**
- **Create a Closed-Loop Control System for Different Motion Parameters**

## What is PWM

Pulse Width Modulation (PWM) is a method used to control analog devices using digital outputs. By switching the power supply on and off at a high frequency, PWM controls the average current flowing to a device.

![PWM Signals](path_to_image/PWM_signals_image.jpg)

### How PWM Works

By switching the power supply to a device on and off at high frequency, the average current flowing through it can be accurately controlled. The duty cycle, or the percentage of time the signal is on, determines the power delivered to the device.

### Duty Cycle

The duty cycle is the percentage of one period in which a signal or system is active. A higher duty cycle corresponds to more power delivered to the device.

## Hardware Required

- **Spartan 6 FPGA Board (XC6SLX16-2FTG256C)**
- **Pocket Science Oscilloscope**
- **DC Motor and Driver**
- **2 Push Buttons (for speed increment and decrement)**
- **L293D Motor Driver**

## Setup and Implementation

1. **FPGA Board Specifications:**
   - On-Board FPGA: XC6SLX16-2FTG256C
   - On-Board FPGA external crystal frequency: 50MHz
   - On-Board M25P80 SPI Flash, 1M bytes for user configuration code
   - On-Board 256MB Micron DDR3, MT41J128M16JT-125
   - Power Supply: 3.3V using MP2359 wide input range DC/DC
   - PCB Size: 6.7cm x 8.4cm

2. **Connect the Hardware:**
   - Connect the DC motor to the L293D motor driver.
   - Interface the motor driver with the FPGA.
   - Connect the push buttons to the FPGA for speed control.

## VHDL Code

```vhdl
-- LIBRARY DECLARATION
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Entity declaration
entity PWMMOTOR is
    Port (
        CLK : in  STD_LOGIC;
        INCREMENT : in  STD_LOGIC; -- Pushbutton for increment
        DECREMENT : in  STD_LOGIC; -- Pushbutton to decrement
        PWM_OUTPUT : out  STD_LOGIC
    );
end PWMMOTOR;

-- Architecture
architecture Behavioral of PWMMOTOR is
    signal count: integer range 0 to 500 := 0;
    signal duty_cycle: integer range 100 to 500 := 100;
    signal i : integer range 0 to 500 := 0;
begin
    process(CLK)
    begin
        if rising_edge(CLK) then
            count <= count + 1;
            if count = duty_cycle then
                PWM_OUTPUT <= '1';
            end if;
            if count = 500 then
                PWM_OUTPUT <= '0';
                count <= 0;
                if (INCREMENT = '1' and duty_cycle < 450) then
                    i <= i + 1;
                    if i = 500 then
                        duty_cycle <= duty_cycle + 1;
                        i <= 0;
                    end if;
                elsif (DECREMENT = '1' and duty_cycle > 50) then
                    i <= i + 1;
                    if i = 500 then
                        duty_cycle <= duty_cycle - 1;
                        i <= 0;
                    end if;
                end if;
            end if;
        end if;
    end process;
end Behavioral;
```

## Demo

- Use the two push buttons to increment and decrement the motor speed.
- The L293D driver is used to drive the motor.

## References

- [Power MOSFET on Wikipedia](https://en.wikipedia.org/wiki/Power_MOSFET)

For more details, refer to the provided project documentation.

---

