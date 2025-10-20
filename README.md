# Overview

A single-class Arduino library for driving a multiplexed 4-digit 7-segment display using two daisy-chained 74HC595 ICs.

## Concept

**74HC595**, sometimes simply called **595**, is a widely used 8-bit serial-in, parallel-out (SIPO) shift register
integrated circuit (IC) commonly employed to drive 7-segment displays.

Two daisy-chained 595s form a 16-bit shift register, which is enough for driving a 4-digit 7-segment display
in such a way that one byte determines which display segments are currently ON and OFF and the other byte
(4 bits out of 8) will control which digits (character positions, character cells) are currently ON and OFF.
Thus first byte is referred to as **`gbyte`** ("glyph byte") and the other one as **`cpbyte`** ("character position byte").

`gbyte` and `cpbyte` may come in any order, that is, in a 16-bit register any one of them may be either upper byte
or lower byte.

Usually via a switching device (most commonly a transistor), since 595's ability to source/sink current for a whole
set of 7 LEDs is close to its electrical limitations. 

common cathode, common anode
common pin

## Multiplexing

Multiplexing is

First byte 

 "glyph byte" ('gbyte')
and "common pin"

In this configuration first IC (first 8 bits) can be used to
turn display segments ON and OFF, thus causing various glyphs to be output. This byte is called "glyph byte",
or `gbyte`. 

for common anode inverted (in a sense that `cpbyte` outputs must connect common pins to the positive rail, not to the ground).


(`cpbyte`) (`gbyte`)

Wiring in your circuit **may** differ from the schematic provided in this README, and the library will still
be applicable. The only premise that must be followed is that one IC is used to turn segments ON and OFF
and the other one controls the common pins of a display.

![Circuit diagram (schematic)](assets/circuit_diagram_(schematic).png)


## Glyphs and byte mapping

Typically, outputting a glyph (a character representation) to a 7-segment display involves custom-forming a byte
whose combination of bit states (set or cleared) corresponds to a pattern in which the segments must be turned
ON and OFF to form a recognizable symbol. Finding the proper correspondence between the bit states and the segment
pattern is called **mapping**.

The **mapped bytes** (sometimes called **bit masks**) can be formed in advance and hard-coded into a program.
Although it may be perfectly acceptable, it may become troublesome if the program needs to be adapted to a circuit
with a different wiring order between the device's outputs and the display's control pins. To simplify and automate
the task, you may want to try [SegMap595](https://github.com/ErlingSigurdson/SegMap595) library that provides
a simple API for byte mapping and retrieving the necessary bytes.

## Compatibility

The library works with any Arduino-compatible MCU capable of bit-banging or SPI data transfer.
