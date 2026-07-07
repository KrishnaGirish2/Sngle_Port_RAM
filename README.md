# Single Port RAM Verification using SystemVerilog

## Overview

This repository contains the verification environment developed for a **Single Port RAM** using **SystemVerilog** (mailbox-based architecture). The verification environment demonstrates the complete verification flow, including random testing, directed testing, functional coverage, reference model comparison, and regression testing.

## Project Objectives

* Verify the functionality of a Single Port RAM.
* Develop a reusable SystemVerilog verification environment.
* Compare DUV outputs with a reference model.
* Identify and fix bugs in the supplied verification environment.
* Achieve 100% functional coverage.

## Verification Environment

The testbench consists of the following components:

* **defines.sv** – Defines global macros such as the total number of transactions.
* **package.sv** – Includes all verification class files.
* **Interface** – Contains separate clocking blocks and modports for the Driver, Monitor, and Reference Model.
* **Transaction** – Defines the stimulus packet containing address, data, control, and response signals.
* **Generator** – Generates randomized and directed transactions.
* **Driver** – Drives transactions from the generator to the DUT through the virtual interface.
* **Monitor** – Samples DUV outputs and forwards the observed transactions to the scoreboard.
* **Reference Model** – Implements a behavioral RAM model to generate the expected output.
* **Scoreboard** – Compares DUV output with the expected output from the reference model.
* **Environment** – Instantiates and connects all verification components using mailboxes and virtual interfaces.
* **Test** – Builds the environment and starts the verification process.
* **Top Module** – Instantiates the DUV, interface, clock/reset generation, and the testbench.

## Verification Flow

Generator → Driver → DUV → Monitor → Scoreboard

Generator → Driver → Reference Model → Scoreboard

The scoreboard compares the DUT output against the reference model output and reports matches or mismatches.

## Testing Performed

### Random Testing

Randomized transactions were generated using constraint-based randomization to verify different combinations of:

* Read operations
* Write operations
* Addresses
* Data values

### Directed Testing

Directed tests were created to verify specific scenarios, including:

* Write followed by read at a predefined address.
* Additional directed write/read transactions for specific address and data combinations.
* Dedicated test cases with constraints forcing:

  * `write_enb = 0` and `read_enb = 0`
  * `write_enb = 1` and `read_enb = 1`

These directed scenarios helped verify corner cases and improve functional coverage.

### Regression Testing

After each bug fix, the complete verification suite was executed to ensure:

* Existing functionality was not affected.
* No new mismatches were introduced.
* Functional coverage remained complete.

## Bugs Identified and Fixed

* Changed transaction variables from **bit** to **logic** to correctly preserve four-state values (`0`, `1`, `X`, and `Z`).
* Fixed missing constructor argument while calling `super.new()` in the extended transaction/reference model.
* Fixed missing constructor argument during object creation in the top/test environment.
* Added missing active-low reset behavior in the reference model so that the expected output matched the DUT.
* Corrected reference model behavior for reset and memory accesses to match DUT functionality.

## Functional Coverage

The verification environment includes functional coverage for:

* Write enable
* Read enable
* Address
* Input data
* Cross coverage of read/write operations
* Output data coverage

After incorporating directed test cases and additional constrained transactions, **100% functional coverage** was achieved.

## Repository Contents

* RTL Design (Single Port RAM)
* Complete SystemVerilog Testbench
* Verification Components
* Reference Model
* Functional Coverage
* Directed Test Cases
* Random Test Cases
* Regression Test
* Verification Report

## Tools Used

* SystemVerilog
* QuestaSim 

## Results

The verification environment successfully validated the functionality of the Single Port RAM. Random and directed testing, reference model checking, functional coverage, and regression testing confirmed correct DUT operation after resolving the identified issues.
