
## Summary

This project is an SOC (System on a Chip) coded in VHDL and implemented for the Lattice iCE40-hx8k dev board. The SOC contains the following components: MSP430 CPU + UART + Timer + I/O Ports

## Required Hardware

* Lattice iCE40-hx8k dev board (can be ordered online at www.latticesemi.com)
* USB-to-Serial 3.3V adapter (can be ordered from eBay)
* misc USB cables and wires for connecting the USB-to-Serial adapter

NOTE: Make sure the USB-to-serial adapter is a 3.3V version. Some adapters have 5V interface signals which could damage your iCE40-hx8k dev board.

## Tools

* IceCube2 (from Lattice Semiconductor) was used for synthesis and FPGA Routing.
* Icestorm (https:/github.com/cliffordwolf/icestorm) was used for programming.


## Build Flow

I used the Lattice IceCube2 software to generate the SOC_bitmap.bin programming file and then I used this command line "iceprog SOC_bitmap.bin" the program the iCE40-hx8k dev board over the USB cable (iceprog is part of the icestorm tool suite).

## Console Interface

I used the minicom program (on Ubuntu Linux) as a console to communicate with the MSP430 SOC over the USB-to-Serial connection. Configure minicom using the command line "minicom -s" to configure the serial port for ttyUSB0 and turn of the hardware handshaking. There are probably other alternatives to minicom. Any ANSI terminal-emulator program should work for this application.

## Pinout

The iCE40 pins are defined as follows:
```
UART_RXD   G1 -pullup yes
UART_TXD   G2
PORTA[0]   B5
PORTA[1]   B4
PORTA[2]   A2
PORTA[3]   A1
PORTA[4]   C5
PORTA[5]   C4
PORTA[6]   B3
PORTA[7]   C3
RESET      N3 -pullup yes
CLK        J3 -pullup yes
```

## Memory Map

The memory map of this SOC is as follows:
```
$0000 -> $0007  UART registers
$0008 -> $000B  Timer registers
$000C           Output port register
$000D           Interrupt mask register
$000E -> 000F   Random number registers
$0010           Interrupt source register
$E000 -> $FFFF  Boot RAM (8k-bytes)
```

## MSP430 CPU Background Info

The MSP430 was first introduced by TI in the mid 2000's and TI has since produced 6 generations of the MSP430 family. The MSP430 CPU has a 16-bit data path and byte-addressable memory (64K bytes with little-endian byte ordering). The MSP430 has 16 registers, 4 of which are special purpose (R0=PC, R1=SP, R2=status/CG1, and R3=CG2). The CG1 and CG2 functions provide for 6 commonly used constant values without requiring an extra word for immediate data. The instruction set is minimalistic with only 27 instructions total (8 jumps, 7 one-operand, and 12 two-operand). Source operands can use 4 addressing modes (Rn @Rn, @Rn+, and x(Rn) and destination operands can use 2 addressing modes (Rn, and x(Rn)). Most instruction (except jumps) have a word/byte bit in the opcode to control the operand width. The combination of addressing modes and word/byte results in a very powerful and comprehensive instruction set. This SOC implements the core MSP430 CPU and does not attempt to emulate any specific TI MSP430 SOC/product.


## Contributors

* Scott L Baker - SOC design

## License

See the **LICENSE** file in this repository
