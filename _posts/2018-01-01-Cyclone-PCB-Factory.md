---
published: false
layout: post
description: Configure Cyclone PCB Factory
tags:
  - 'PCB, Cyclone'
---
<center><img src="/images/CNCPCBFactory.jpg" width="458" height="458"></center>
The aim of this tutorial is showing how to configure a Cyclone PCB factory. 

<!-- more -->

<h2>First steps</h2>
The main circuit to control the CNC is an Arduino with an Arduino CNC Shield.

<h3>Arduino CNC Shield</h3>
The Arduino CNC Shield makes it easy to get your CNC projects up and running in a few hours. It uses opensource firmware on Arduino to control 4 stepper motors using 4 A4988 Stepper drivers, with this shield and one Arduino.

<h4>Jumper Settings</h4>

Jumpers are used to configure the 4th Axis, Micro stepping and endstop configuration.
The next few sections explains how its done.

<h4>4th Axis Configuration</h4>

Using two jumpers the 4th axis can be configured to clone the X or Y or Z axis. It can also run as an individual axis by using Digital Pin 12 for Stepping signal and Digital Pin 13 as direction signal. (GRBL only supports 3 axis’s at the moment)

Clone X-Axis to the 4th stepper driver.
<center><img src="/images/CNCShieldCloneXAxis.jpg" width="167" height="142"></center>

Clone Y-Axis to the 4th stepper driver.
<center><img src="/images/CNCShieldCloneYAxis.jpg" width="167" height="142"></center>

Clone Z-Axis to the 4th stepper driver.
<center><img src="/images/CNCShieldCloneZAxis.jpg" width="167" height="142"></center>

Use D12 and D13 to drive the 4th stepper driver.
<center><img src="/images/CNCShieldCloneZAxis.jpg" width="167" height="142"></center>

<h4>End Stop Configuration</h4>

By default GRBL is configured to trigger an alert if an end-stop goes low(Gets grounded). On the forums this has been much debated and some people requested to have active High end-stops. The jumpers in the picture provides the option to do both. (To run with default setting on GRBL the jumper need to be connected like the left shield in the image below)(This Jumper was only introduced in Version 3.02)
<center><img src="/images/CNCShieldEndstopConfiguration.jpg" width="445" height="543"></center>

End-stop switches are standard “always open” switches. An End-stop gets activated when the end-stop pin connects to ground(When setup with default GRBL settings).

<h4>Configuring Micro Stepping for Each Axis</h4>

Each axis has 3 jumpers that can be set to configure the micro stepping for the axis.
<center><img src="/images/CNCShieldMicroSteppingSettings.jpg" width="686" height="550"></center>

In the tables below High indicates that a Jumper is insert and Low indicates that no jumper is inserted.

Pololu A4988 Stepper Driver configuration:
|MS0 	|MS1 	|MS2 	|Microstep Resolution|
|-------|--------|---------|---------|
|Low 	|Low 	|Low 	|Full step|
|High 	|Low 	|Low 	|Half step|
|Low 	|High 	|Low 	|Quarter step|
|High 	|High 	|Low 	|Eighth step|
|High 	|High 	|High 	|Sixteenth step|

Pololu DRV8825 Stepper Driver configuration:
|MODE0 	|MODE1 	|MODE2 	|Microstep Resolution|
|-------|--------|---------|---------|
|Low 	|Low 	|Low 	|Full step|
|High 	|Low 	|Low 	|Half step|
|Low 	|High 	|Low 	|1/4 step|
|High 	|High 	|Low 	|1/8 step|
|Low 	|Low 	|High 	|1/16 step|
|High 	|Low 	|High 	|1/32 step|
|Low 	|High 	|High 	|1/32 step|
|High 	|High 	|High 	|1/32 step|

<h3>References</h3>
1. <a href="https://blog.protoneer.co.nz/arduino-cnc-shield-v3-00-assembly-guide/" target="_blank">Arduino CNC Shield</a>