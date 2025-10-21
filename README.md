# Overview

A single-class Arduino library for driving a multiplexed 4-digit 7-segment display using two daisy-chained 74HC595 ICs.

## Concept

A typical 4-digit 7-segment display has 12 input pins, therefore driving it requires 12 individual signals:
* 8 signals turn ON or OFF individual segments.
* 4 signals turn ON or OFF whole digits (character positions).

Driving such a display using a microcontroller unit (MCU) commonly involves extending its own digital outputs
via an interfacing device, such as a **74HC595** IC.

74HC595, sometimes simply called **595**, is a widely used 8-bit serial-in, parallel-out (SIPO) shift register
integrated circuit (IC) commonly employed to drive 7-segment displays. Multiple 595s can be daisy-chained: connected
in such a way that the serial output of one IC (despite being a SIPO register, it does have a supplementary digital
output) goes to the serial input of the next one and all ICs in the chain receive same clock and latch signals.
Two daisy-chained 595s form a 16-bit shift register, which is sufficient for controlling a 12-input display.

This library assumes that 

Within this approach one IC in the chain (one byte) determines which display segments are currently ON and OFF and the other byte
(4 bits out of 8) determines which digit (character position) is currently ON and OFF. First byte is referred to
as **`gbyte`** ("glyph byte") and the other one as **`cpbyte`** ("character position byte"). `gbyte` and `cpbyte`
may be shifted into a 16-bit register in any order, that is, any one of them may be either upper byte or lower byte.

Four bits of `cpbyte` which are used to switch character positions do so by connecting and disconnecting the display's
common pin to a ground (in case a common cathode display) or to a positive rail (in case of a common anode display).
Usually it is done via a switching device (most commonly a transistor), since 595's ability to source/sink current
itself for a whole set of 7 LEDs is close to its electrical limitations.

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

## API usage


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
