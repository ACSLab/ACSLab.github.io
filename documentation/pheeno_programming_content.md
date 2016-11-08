---
layout: default
title: Beginning to Program Pheeno
---
One of the processors on board the Pheeno robotic platform is the Arduino Pro Mini with the ATMega328 (3.3V, 8MHz) processor. This section of the guide will introduce how the sensors and motors on the robot are controlled by this processor. It will also walk you through small codes to make the robot move around and sense its environment.

<!-- This guide will focus heavily on premade libraries and code available on the [GitHub](https://github.com/ACSLab/PheenoRobot/) repository for Pheeno. These codes and libraries are not necessary for the operation of Pheeno in the long run but do allow for easier coding with subroutines for making the robot drive around using open and closed loop control. If you are new to Arduino programming and general electronics, Section \ref{sec:ArduinoPinInfo} may not be very informative and can be skimmed over. If you are experienced in Arduino coding, Section \ref{sec:ArduinoPinInfo} gives details on how the sensors and motors are connected to the Arduino so you can develop your own scripts. If you run into issues with programming the Arduino, there are plenty of resources available at the [Arduino Forums] (https://forum.arduino.cc/) and specifically on the [Arduino Pro Mini] (https://www.arduino.cc/en/Guide/ArduinoProMini/). Of course, always feel free to contact the e-mail above with any questions or issues! -->

## General Pin Information for Connection between the Arduino and Motors/Sensors
<!-- If you want to create your own code and bypass the premade libraries this section will explain briefly how Pheeno's sensors and motors are connected to the Arduino! Table \ref{tab:ArduinoCon} gives a general overview of how the pins of the Arduino Pro Mini are connected to the various sensors and actuators. -->

<style>
table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
}

td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
}

tr:nth-child(even) {
    background-color: #dddddd;
}
</style>

<table>
    <tr>
        <th>Component</th>
        <th>Connected Pins</th>
        <th>Pin Type</th>
    </tr>
    <tr>
        <td> Left IR Range Sensor </tr>
        <td> A2 </tr>
        <td> Analog </tr>
    </tr>
    <tr>
        <td> Left Forward IR Range Sensor </td>
        <td> A1 </td>
        <td> Analog </td>
    </tr>
    <tr>
        <td> Center IR Range Sensor </td>
        <td> A0 </td>
        <td> Analog </td>
    </tr>
    <tr>
        <td> Right Forward IR Range Sensor </td>
        <td> A6 </td>
        <td> Analog </td>
    </tr>
    <tr>
        <td> Right IR Range Sensor </td>
        <td> A7 </td>
        <td> Analog </td>
    </tr>
    <tr>
        <td> Back IR Range Sensor </td>
        <td> A3 </td>
        <td> Analog </td>
    </tr>
    <tr>
        <td> LSM303D (Accelerometer/Magnetometer) </td>
        <td> A4, A5 </td>
        <td> SDA, SCL </td>
    </tr>
    <tr>
        <td> Left Encoder </td>
        <td> 3, 12 </td>
        <td> Digital Interrupt, Digital </td>
    </tr>
    <tr>
        <td> Right Encoder </td>
        <td> 2, 13 </td>
        <td> Digital Interrupt, Digital </td>
    </tr>
    <tr>
        <td> Left Motor </td>
        <td> 5, 6, 7 </td>
        <td> Digital PWN, Digital, Digital <td>
    </tr>
    <tr>
        <td> Right Motor </td>
        <td> 11, 9, 10 </td>
        <td> Digital PWM, Digital, Digital </td>
    </tr>
    <tr>
        <td> Camera Servo </td>
        <td> 4 </td>
        <td> Digital </td>
    </tr>
</table>

The IR range sensor data is relatively straight forward. The analog voltage input is taken in through the 10 bit ADC converter measured from ground to VCC. The conversion from voltage to distance is fairly linear until the extremes of the sensor. An example conversion can be found in the readIR function of Pheeno.cpp. This conversion was found experimentally so feel free to find your own conversion if you find it is too inaccurate.

The LSM303D is a board within Pheeno that contains an accelerometer and magnetometer. It sends data to the Arduino through I2C communication. Some example files on how this communication is done are in the LSM303D library. Additional information can be found through a supplier of the board, [Pololu] (https://www.pololu.com/product/2127).

The left and right encoders make use of the external interrupt pins available on the Arduino Pro Mini. These encoders can be used as normal encoders with 6 counts per revolution only using the sensors connected to the interrupt pins. They can also be used as quadrature encoders using both sets of pins. The encoder library supplied is highly recommended for quadrature encoder use.

The left and right motor are controlled using PWM control for a pseudo analog signal and a H-bridge motor board to control the direction. Pins 6 and 7, for the left motor, and 9 and 10, for the right motor, may be driven to opposite voltages (HIGH and LOW) to determine the direction of motion or both HIGH to "short" the motor providing a brake. Pins 5 and 11 are PWM pins that determine the speed the motor.  The Arduino uses a range of 0-255 to regulate the duty cycle of the PWM signal. Subroutines forwardL, forwardR, reverseL, reverseR, brake, and noMotion in the provided Pheeno arduino library (Pheeno.h, Pheeno.cpp) may be viewed as example motor functions.

The camera servo is controlled by pin 4. The servo library is highly recommended for its use.

## Getting Started!
