---
title: Considerations for Short Distance UART Communication, Part 1
tags: [robotics]
---

Whenever a system consists of multiple processors, the typical question
that arises very early into the design process is that of their mutual
communication. I've been programming chips to communicate for years now
and went though quite a few communication protocols. Lately, I've been doing
a lot of thinking on communication protocol design and I'd like to share
my thoughts.

This is the first episode of what hopefully will become
a series of articles, in which I explain the resoning behind the protocols
that my friends and I use for communication between processors within
a single device. I'll discuss the problems that we encountered on the way,
and how we solved them (or rather how I believe we should solve them).

I will make a big leap right away and state that robotics enthusiasts
put processors into their robots not because they need a lot of computing
power, but rather to control the robot's peripherals (e.g. motors).
After all, if they needed to compute, they would just embed
a notebook into the robot. Who would want to to port their neural
networks to an embedded ARM?

I personally used to work with PICs and switched to AVRs shortly after.
These are 8-bit microcontrollers (MCUs) that require very few external
components to run, they are relatively cheap, and mostly they get the
job done effectively. They are, however, somewhat limited when it comes to
speed (typically around 10 to 20 MIPS) and memory (as low as 512 bytes).
The strength of these units is in the hardware peripherals they
usually come with: PWM channels, A/D converters, and---you guessed
it---communication interfaces.

While there are I2Cs, SPIs, CANs, USBs and other TLAs, the interface
that I believe is the most used is the UART. The advantages of
this interface are plenty.

 * Almost every MCU has a hardware UART interface and the interface
   is extremely simple to control from software (unlike I2C or,
   perish the thought, USB).
 * Multiple UARTs can typically be found on an MCU (AVR XMEGA A has five),
   unlike other types of interfaces (save maybe for SPI) of which
   there usually is one, if any.
 * It can be connected to a PC via the RS232 port with only
   a simple level adjustment (and there are dedicated chips for it,
   notably MAX232).
 * There is an abundance of comm modules that can bridge UART
   over to other kinds of channels, including USB, Bluetooth,
   and ZigBee (making it extremely easy to communicate wirelessly).
 * You can bend UART to your needs. Turn it to a wired-AND bus
   if you are limited in the number of wires, for example.

Now before I continue, let me say that UART is not the best option
for long-distance communication. A while back, during the [Eurobot
Starter][1] competition, one of our teams controlled their robot
from a panel, that was connected using a 6 meter, 3 wire UART cable
(GND, TX, RX). It turned out to be a really bad idea, as the cross talk
between the wires was so strong that the two data stream simply corrupted
each other. (Actually, now that I think about it, there probably were
series resistors at the both ends of each wire to prevent an accidental
short. That certainly didn't help either.)

In any case, UART is what we've been using forever now. I dare you to
show me a better (in terms of what I've written above) solution.
In the next episode, I will outline the usual requirements that
we have to put on comm protocols in embedded devices and discuss the
protocol that we've been using as a result of those requirements.

  [1]: http://www.eurobot.cz/
