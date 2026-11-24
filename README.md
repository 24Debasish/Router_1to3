# 1x3 Router Design

**A packet-based Layer-3 router that routes incoming network packets to one of three destination channels based on the address information in the packet header.**

## Objective

The objective of this project is to design and implement a 1x3 Router capable of receiving packets from a source LAN and forwarding them to the appropriate destination LAN. The design ensures efficient packet routing, temporary packet storage, synchronization, and error detection using parity checking.

## Project Overview

A router is an OSI Layer-3 networking device that forwards data packets between computer networks based on the destination address contained in the packet header.

The Router 1x3 design follows a packet-based communication protocol and receives packet data byte-by-byte on every positive edge of the clock. The start of a packet is indicated by the `pkt_valid` signal, while the end of the packet is indicated by the de-assertion of `pkt_valid`.

The router contains three dedicated output channels and three corresponding FIFOs. Incoming packets are stored in the FIFO selected according to the destination address field. Destination LANs monitor valid output signals and retrieve packets using dedicated read-enable signals.

To ensure data integrity, the design implements parity-based error detection. If a mismatch occurs between the transmitted parity byte and the internally calculated parity, an error signal is generated. The router also supports a busy mechanism that prevents the source from transmitting data when the router is occupied.

## Architecture and Design Components

The Router 1x3 architecture consists of the following major blocks:

### 1. Finite State Machine (FSM)

- Controls packet processing operations.
- Manages packet loading and routing states.
- Generates control signals for router functionality.

### 2. Synchronizer

- Decodes destination addresses.
- Generates write-enable signals for FIFOs.
- Monitors FIFO status signals.
- Handles soft-reset generation.

### 3. Register Block

- Stores packet header and payload information.
- Calculates parity.
- Verifies received packet parity.
- Generates error indication signals.

### 4. FIFO Buffers

The design contains three dedicated FIFOs:

- FIFO_0
- FIFO_1
- FIFO_2

Each FIFO stores packets corresponding to a specific destination channel and supports independent read and write operations.

### Packet Flow

1. Source LAN sends packet data through `data_in`.
2. Router receives packet bytes sequentially.
3. Destination address is decoded.
4. Appropriate FIFO is selected.
5. Packet is stored in the selected FIFO.
6. Destination LAN monitors `vld_out_x`.
7. Packet is read using `read_enb_x`.
8. Data is transmitted through `data_out_x`.
9. Parity checking validates packet integrity.
10. Error signal is generated if parity verification fails.

## Tools and Technologies

### Hardware Description Language

- Verilog HDL

### Digital Design Concepts

- Finite State Machine (FSM)
- FIFO Architecture
- Packet-Based Communication
- Synchronization Logic
- Parity Error Detection
- Sequential Logic Design
- Active-Low Synchronous Reset

### Functional Features

- 1x3 Packet Routing
- Three Independent Output Channels
- FIFO-Based Buffering
- Busy Signal Handling
- Error Detection Using Parity Checking
- Simultaneous Output Packet Reading

## Author

**Debasish Ghosh**
