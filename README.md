# NEC Protocol Transmitter-Receiver Logic Implementation on SLG47011

## Abstract

This project describes the SLG47011 configured as a NEC protocol signal controller.

## Introduction

Infrared (IR) communication is widely used in consumer electronics for wireless control, with the NEC protocol being one of the most popular IR transmission standards. It is characterized by its simple pulse-distance modulation, reliability, and wide adoption in remote control systems.

In this project, I aim to implement NEC IR remote protocol control logic on the SLG47011 and test its operation in practice. The goal is to design and develop a working prototype that can both transmit and receive IR signals according to the NEC specification. The prototype will be used to validate the correctness of timing, signal structure, and overall protocol compliance. To verify the performance of the device, it will be tested with real consumer electronics that use the NEC protocol, allowing for a direct comparison of functionality and behavior.

## NEC Infrared Transmission Protocol

The NEC IR transmission protocol uses pulse distance encoding to represent message bits. Each pulse burst (mark — RC transmitter ON) lasts 562.5µs and is modulated at a carrier frequency of 38kHz (with a period of 26.3µs). Bits are encoded as follows:

- **Logical '0'** — a 562.5µs pulse burst followed by a 562.5µs space, for a total bit time of 1.125ms
- **Logical '1'** — a 562.5µs pulse burst followed by a 1.6875ms space, for a total bit time of 2.25ms

### Message Structure

When a key is pressed on the remote controller, the transmitted message includes:

- 9ms leading pulse burst
- 4.5ms space
- 8-bit address for the receiving device
- 8-bit logical inverse of the address
- 8-bit command
- 8-bit logical inverse of the command
- Final 562.5µs pulse burst to signify the end of transmission

The bits in each byte are transmitted least significant bit (LSB) first.

### Repeat Codes

When the key on the remote controller is held down, a repeat code is transmitted approximately 40ms after the pulse burst that indicated the end of the previous message. This repeat code is then sent continuously at intervals of 108ms until the key is released. The structure of the repeat code is as follows:

- 9ms leading pulse burst
- 2.25ms space
- 562.5µs pulse burst to mark the end of the repeat code

## NEC IR Remote Protocol Alternatives

There are some alternatives to the NEC IR remote protocol on the market, such as:

- Sony SIRC
- RC-5
- RC-6
- Panasonic IR Protocol
- Sharp IR Protocol

NEC protocol remains one of the most widely used IR remote control protocols due to its simplicity, robustness, and wide adoption in consumer electronics.

## Project Phase 1: Transmitter

At the end of the 1st phase of the project we now have a complete, working transmitter prototype, capable of transmitting messages in compliance with the NEC IR Protocol.

The transmitter's operating principle is based on combination function macrocells, multi-function macrocells and Memory Table block with internal oscillator configured to forward IR signals with 38.2 kHz carrier frequency.

## Project Phase 2: Receiver

Phase 2 of the project was dedicated to the development of a NEC IR Protocol receiver device, designed to operate synchronously with the previously constructed transmitter prototype. The primary goal of this stage was to establish a complete, bidirectional test bench for protocol validation.

The core operating principle of the receiver prototype relies on decoding the precise timing intervals of the incoming IR signal. The receiver utilizes an NXOR Look-Up Table (LUT) to perform hardware pattern matching. This mechanism compares the decoded bit stream against pre-configured message templates stored within the internal Memory Table block.

The current design successfully supports the identification and processing of up to seven distinct NEC command patterns. Upon a successful match, the prototype provides visual feedback: one of seven dedicated LEDs illuminates, corresponding directly to the recognized command channel.

By the conclusion of Phase 2, a fully functional NEC IR Protocol message decoder was achieved. Its successful operation in tandem with the transmitter device validates the correctness of the signal timing logic across both the encoding and decoding processes.

## References

1. "NEC Infrared Transmission Protocol," 2013. [Online]. Available: [https://sibotic.wordpress.com/wp-content/uploads/2013/12/adoh-necinfraredtransmissionprotocol-281113-1713-47344.pdf](https://sibotic.wordpress.com/wp-content/uploads/2013/12/adoh-necinfraredtransmissionprotocol-281113-1713-47344.pdf)

2. "Demonstrating the Popular NEC Infrared Protocol," 2025. [Online]. Available: [https://www.electronicdesign.com/technologies/communications/iot/article/55331179/renesas-electronics-using-the-slg47011-to-implement-the-nec-infrared-protocol](https://www.electronicdesign.com/technologies/communications/iot/article/55331179/renesas-electronics-using-the-slg47011-to-implement-the-nec-infrared-protocol)
