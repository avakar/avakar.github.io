---
title: ZigBit ATZB-24-A2 Pinout
tags: [zigbit, robotics]
---

I've spent the last Friday afternoon trying to get my radio controlled airplane talking to my transmitter via a ZigBee link. I've deemed the traditional RF connection utterly useless as I'm getting rather severe glitches as close as 15 meters away (maybe I shouldn't have tinkered with the receiver, who knows).

My friend and I bought four ZigBit modules a few months ago, but I didn't find the time to work on them until last week. The BitCloud stack provided in the SDK is surely impressive, though unsuitable for real-time control. I basically want an isochronous transfer, so I had to build the software from scratch.

There are two important chips inside the module, atmega1281 processor and at86rf230 ZigBit tranceiver. There is unfortunately no documentation from Atmel about how the chips are interconnected or how the module's pads map to the processor's pins, so I've had to map the pinout myself. Here it is.

![ZigBit ATZB-24-A2 Pinout](/images/zigbit_mcu_pinout_mapping_0.svg)

Most of the pins are either supply pins or are directly connected to the embedded atmega1281 processor. There are two notable exceptions.

 * The RF_CLKM pin is connected to the CLKM pin of the transceiver and outputs 1MHz clock by default. It is possible to disable it or to reprogram it to output either 2, 4, or 8MHz.
 * The OSC32K is connected to the output of a 32.786kHz oscillator and is also internally connected to the processor's TOSC1 pin (and can therefore serve as a clock source for the 8-bit asynchronous timer).

## Internal connections

I couldn't map the connection between the processor and the transceiver directly, but some mapping is obvious (they communicate through the SPI lines, which means that MISO, MOSI and SCK are connected) and I've extracted the rest from the BitCloud sources.

atmega1281 | at86rf230
-----------|----------
PB0        | CS
PB1/SCK    | SCK
PB2/MOSI   | MOSI
PB3/MISO   | MISO
PB4        | SLP_TR
PA7        | ~RST
PE5/INT5   | IRQ

## In-system programming

As you might have noticed, the in-system programming interface (SCK, PDO, PDI, RESET) is accessible from outside. According to one of the readers (thank you, Joerg Wunsch!) the chip can indeed be programmed using the normal in-circuit programmer, so if you own one, give it a go!

