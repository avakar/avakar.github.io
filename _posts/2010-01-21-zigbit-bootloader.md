---
title: ZigBit Bootloader
tags: [zigbit, robotics]
---

I was playing with Atmel's ZigBit ATZB-24-A2 over the last few days and wrote a command-line utility that runs on both Windows and Linux and can be used to flash programs into these modules (assuming that they have the original bootloader installed). Atmel distributes their own bootloader client as part of the BitCloud SDK, but the command-line version didn't work for me at all and the GUI version was very unreliable.

The program accepts two parameters, the name of the serial port and the path to the .srec file containing the program and eeprom values to be flashed.

    zigbit_bootloader <serial-port> <srec-file-path>

You can pass any file as a serial port, as long as it supports your platform's comm API (`SetCommParameters` and `SetCommTimeouts` on Windows, `tcsetattr` et al. on Unix). Example paths: `COM1` (Windows), `\\.\COM25` (Windows), `/dev/ttyS1` (Unix), `/dev/rfcomm0` (Unix).

The second parameter names a [SREC file][1]. You can convert Intel's HEX file to SREC using `objcopy`. Be sure to use `--srec-len=128` (128 is the size of a program memory page on atmega1281). Note that the program performs no integrity checking on the SREC file, make sure that the file is complete and unchanged before starting the flash procedure.

  [1]: http://en.wikipedia.org/wiki/SREC_%28file_format%29

You must restart the device after starting this flash utility. The bootloader in the device is only active for the first 500 milliseconds. Unless activated within that time period, the control is transferred to the main program.

## The protocol

The protocol that the Atmel's bootloader is documented in the SDK, but beware that the documentation is very much wrong. The good news is that the sequences listed in the document are correct:

    HANDSHAKE_REQ: 0xB2, 0xA5, 0x65, 0x4B
    HANDSHAKE_CONF: 0x69, 0xD3, 0xD2, 0x26
    ACK: 0x4D, 0x5A, 0x9A, 0xB4
    NACK: 0x2D, 0x59, 0x5A, 0xB2

Once the device is rebooted, the bootloader runs and waits on the serial line for the `HANDSHAKE_REQ`. If it doesn't receive the sequence within 500ms, the bootloader disables the serial line and transfers control to the main program. If the sequence is received in time, the device responds with `HANDSHAKE_CONF`.

The device will now accept the SREC records. Each record is processed and acknowledged by the ACK sequence. Note that especially for EEPROM records, the processing time can be quite high (up to a whole second, if not above).

If the next SREC record isn't received within a few seconds of sending ACK (or HANDSHAKE_CONF in case of the first SREC record), the bootloader will start the program, i.e. there is no sequence sent to shut down the bootloader.

Now, the format of the binary SREC frames is documented incorrectly. Note that each SREC source line has the following format.

    S <decimal digit> <even number of hex digits>

Each frame begins with the `S` and the *decimal* digit (indicating the type of the record). I cannot stress this enough: the first two characters are copied to the output frame without modification.

The rest of the line is then converted to binary -- each pair of hex digits is turned into a single byte. Everything on the line is sent, including the final checksum.

For some reason, the Atmel's Java-based bootloader client sends 256-byte frames containing the SREC record (in the above format) and padded with zeros. I do not fully understand why, the bootloader in the device does not require this. Furthermore, since the bootloader jumps to the start of the application as soon as one of the S7/S8/S9 records is received, it is the application that receives the padding zeros.

The version 0.1 of my client performs this padding (I wasn't aware it wasn't necessary), I've removed the padding a few commits later.

## Building and installing

For Windows, you can download the binary release below.

Otherwise you will have to build from sources. You will need a C++ compiler and the [Boost libraries][2]. You can download the sources below. On Linux, just run `make`, a binary should be created. On Windows, use the Visual Studio project.

  [2]: http://www.boost.org/

## Releases

 * v0.2: [src](/hg/zigbit_bootloader/archive/8808a7b61dea.tar.gz), [win](/files/2010-01-21-zigbit-bootloader/zigbit_bootloader-0.2.zip), [repo](/hg/zigbit_bootloader/rev/8808a7b61dea)
   * No longer sends zero-padded frames.

 * v0.1: [src](/hg/zigbit_bootloader/archive/de1e41b3d824.tar.gz), [win](/files/2010-01-21-zigbit-bootloader/zigbit_bootloader-0.1.zip), [repo](/hg/zigbit_bootloader/rev/de1e41b3d824)
   * Initial release.
   * Compilable on both Windows and Linux
   * Doesn't check the integrity of the source .srec file.
