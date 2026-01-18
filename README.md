# UART Protocol Verification Using SystemVerilog

## Overview

In this project, I implemented a **SystemVerilog-based self-checking verification environment** for a **UART (Universal Asynchronous Receiver–Transmitter)** design.  
The objective is to verify correct UART **transmit (TX)** and **receive (RX)** functionality using a modular, class-based testbench architecture.

The verification flow is fully automated, covering stimulus generation, DUT driving, monitoring, and result checking.

---

## Design Under Test (DUT)

The DUT is a parameterized UART design composed of:

- `uarttx` – UART transmitter  
- `uartrx` – UART receiver  
- `uart_top` – Top-level wrapper integrating TX and RX  

### DUT Features

- Parameterized clock frequency and baud rate  
- Internal UART clock generation (`uclk`)  
- Bit-serial TX and RX operation  
- Completion signals:
  - `donetx` – TX operation complete  
  - `donerx` – RX operation complete  

---

## Verification Architecture

The verification environment is written using **SystemVerilog classes** and follows a **transaction-level methodology**.

### Components

#### Transaction
- Represents a single UART operation  
- Randomized operation type:
  - `write` → TX operation  
  - `read` → RX operation  
- Contains transmit and receive data fields  

#### Generator
- Randomly generates UART transactions  
- Controls the total number of test iterations  
- Synchronizes with driver and scoreboard using events  

#### Driver
- Drives stimulus to the DUT via a virtual interface  
- TX mode:
  - Drives `dintx` and `newd`  
- RX mode:
  - Drives serial data on `rx` bit-by-bit  
- Sends expected data to the scoreboard  

#### Monitor
- Passively observes DUT signals  
- Reconstructs UART data from TX and RX paths  
- Sends observed data to the scoreboard  

#### Scoreboard
- Compares expected data from the driver with actual data from the monitor  
- Reports **DATA MATCHED** or **DATA MISMATCHED**  
- Implements the self-checking mechanism  

#### Environment
- Instantiates and connects all verification components  
- Handles reset, test execution, and simulation completion  
- Coordinates synchronization using events and mailboxes  

---

## Verification Flow

1. DUT reset and initialization  
2. Random UART transaction generation  
3. Driver applies TX or RX stimulus  
4. Monitor captures UART activity  
5. Scoreboard performs data comparison  
6. Simulation ends after all transactions complete  

---

## Key Features

- Class-based SystemVerilog testbench  
- Self-checking scoreboard  
- Randomized UART read and write transactions  
- Mailbox-based inter-component communication  
- Event-based synchronization  
- Support for both UART TX and RX paths  
- Parameterized DUT  
- VCD waveform generation for debugging  

---

## Simulation & Debug

- Console logs display:
  - Generated operations  
  - Data transmitted and received  
  - Scoreboard comparison results  
- Waveforms are dumped to `dump.vcd` for signal-level analysis  
