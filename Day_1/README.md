# Day-1 Basics of NMOS Drain(id) Vs Drain-to-source Voltage(Vds)

A comprehensive guide to understanding CMOS transistor behavior, circuit analysis, and practical SPICE simulation using Sky130 technology.

---

### Introduction to Circuit Design & SPICE Simulations

**Why SPICE Simulations Are Important**
SPICE simulations are a vital tool for designing and analyzing electronic circuits before building physical prototypes. They allow engineers to:

- Predict circuit behavior accurately.
- Identify potential design flaws early.
- Optimize performance without costly or time-consuming hardware.

SPICE supports analog, digital, and mixed-signal circuits, providing insights into voltage, current, power, and signal behavior under different operating conditions.

---

### Overview

This workshop provides an introduction to CMOS transistor operation, SPICE simulation, and characterization of Sky130 devices.
CMOS design depends on NMOS and PMOS transistors operating together to form logic gates.
Understanding their electrical properties is the foundation for analyzing circuit behavior.
You will learn:
- How NMOS transistors respond to different Vgs and Vds conditions.
- How to simulate transistor behavior using SPICE.
- How delay and timing are measured and analyzed in real integrated circuits.

---

### Why SPICE Is Important in Circuit Design

In modern VLSI and analog design, even small mistakes can become very costly once a chip is fabricated. Since manufacturing an IC is expensive, SPICE simulations are essential tools that help designers:
- Test circuits virtually before building hardware.
- Verify the functionality of individual components, like logic gates or amplifiers.
- Analyze performance parameters, including delay, power consumption, and gain.
- Catch design flaws early, saving both time and money.
- Optimize device sizes (W/L ratios) to achieve better performance and lower power usage.

In short, SPICE acts as a bridge between theoretical designs and real hardware, allowing engineers to ensure their circuits will work correctly before fabrication.

---

### Role of SPICE in Circuit Design

SPICE (Simulation Program with Integrated Circuit Emphasis) allows designers to **accurately model and analyze circuit behavior before fabrication**, helping reduce errors and optimize performance.

| Function             | Benefit                               |
|----------------------|--------------------------------------|
| Functional Verification | Ensures logic correctness             |
| Timing Analysis        | Measures propagation delays          |
| Power Estimation       | Calculates static and dynamic power  |
| Design Optimization    | Adjusts transistor sizing and topology |

---

### SPICE Simulation Results

<img width="881" height="743" alt="image" src="https://github.com/user-attachments/assets/42f3c5ca-a569-410b-a765-6c68d775cff0" />

The top graph shows the current-voltage (I–V) characteristics of a CMOS inverter. It illustrates how the drain-source current (Ids) varies with the output voltage (Vout) for different input voltages (Vin), helping analyze the current behavior of the inverter under various conditions.

The bottom graph presents the voltage transfer characteristic (VTC) of the CMOS inverter. It shows how the output voltage (Vout) changes with respect to the input voltage (Vin), providing insight into the inverter’s switching behavior, logic threshold, and noise margins.

---
### Delay Modeling and Analysis

#### Understanding Delay in Digital Circuits

In digital circuits, delay refers to the time it takes for an output to respond after an input changes.

Accurate delay modeling is crucial to ensure that signals arrive at all parts of the chip at the right time, preventing setup and hold violations and ensuring reliable circuit operation.

--- 

### Threshold Voltage with Positive Substrate Bias

In an n-channel MOSFET, applying a positive voltage to the substrate (body) widens the depletion region. This means a higher gate voltage is needed to create strong inversion in the channel. As a result, the threshold voltage increases compared to the case with zero substrate bias. This phenomenon is known as the body effect.

**Some Important Terms**

**Strong Inversion:**
Strong inversion occurs when the gate voltage exceeds the threshold voltage. In this state, a large number of electrons accumulate under the gate oxide, forming a conducting n-channel. This allows significant current to flow from the source to the drain.

**Subthreshold Conduction:**
Subthreshold conduction happens when a MOSFET leaks a small current even if the gate voltage is below the threshold voltage. This is due to the movement of minority carriers across the channel. The current increases exponentially as the gate voltage approaches the threshold.


---

### Cell Delay Dependency
In digital timing analysis, cell delay is not a fixed number.
The delay of a logic cell depends mainly on two factors:

**Input Slew:** The rate at which the input signal changes (transition time).
Slower slews cause longer delays.
**Output Load:** The capacitive load driven by the output.
Larger loads require more time to charge/discharge, increasing delay.

<img width="1253" height="710" alt="image" src="https://github.com/user-attachments/assets/0bb34b34-a7a0-4c19-9169-ec4d6fb7773e" />

**Output Capacitance Formula**
C_total(G1) = C_out(G1) + Σ C_in(connected gates) + Σ C_wire

<img width="1252" height="714" alt="image" src="https://github.com/user-attachments/assets/f4669267-1cce-4ea4-9b4d-a6680aefe6a2" />

## Delay Calculation Using Lookup Tables (LUTs)

In real-world **timing analysis**, the exact input slew and output load values rarely match the entries in a cell’s delay lookup table (**LUT**).  
To determine the delay for intermediate values, **linear interpolation** is commonly used.

### Example
We want to estimate the delay for **CBUF1** when:

- **Input Slew:** 40 ps  
- **Output Load:** 60 fF  

Since **60 fF** isn’t directly listed in the LUT, we refer to the two nearest data points:

| Output Load (fF) | Delay |
|------------------:|:------|
| 50 fF | x₉ |
| 70 fF | x₁₀ |

To estimate the delay at **60 fF**, we perform **linear interpolation** between `x₉` and `x₁₀`:

```math
Delay_{60fF} = x_9 + \left[\frac{(60 - 50)}{(70 - 50)}\right] \times (x_{10} - x_9)
```

<img width="1226" height="726" alt="image" src="https://github.com/user-attachments/assets/b1de8307-e750-4dc3-b9bd-8d2439d51877" />

### Delay Extraction Example — CBUF2

In this example, we aim to determine the **delay for CBUF2** under the following conditions:

- **Input Slew:** 60 ps  
- **Output Load:** 50 fF  

Since **50 fF** is explicitly listed in the lookup table (LUT), no interpolation is required.  
We can directly read the delay value from the table.

From the **CBUF2 delay table**:

| Parameter | LUT Entry |
|------------|------------|
| Input Slew | 60 ps |
| Output Load | 50 fF |
| Delay Value | **y₁₅** |

**Result:**  
The delay for **CBUF2** with an input slew of 60 ps and output load of 50 fF is **y₁₅**, directly taken from the LUT.

Purpose: Estimate delays for intermediate load/slew values
Method: Linear interpolation between nearest LUT entries
Use Case: Timing analysis in STA or SPICE-based delay modeling

---

## Introduction to Basic Element in Circuit Design - NMOS Transistor

### Understanding the NMOS Transistor

An n-Channel MOSFET (NMOS) has four main parts:
**Source & Drain**: Two heavily doped n⁺ regions that let electrons flow when the transistor is on.
**Gate**: The control terminal, usually made of polycrystalline silicon, where the input voltage is applied to turn the device on or off.
**Gate Oxide**: A very thin insulating layer (SiO₂) between the gate and the substrate. It allows the gate to control the channel with an electric field, without any DC current flowing.
**Body (B)**: The p-type substrate that forms the base of the transistor. It affects the transistor’s threshold voltage and overall behavior.

---

NMOS operates by inducing an inversion layer when Vgs > Vth, allowing electrons to flow from source to drain.

<img width="1195" height="502" alt="image" src="https://github.com/user-attachments/assets/9162b112-4d77-4b54-81a3-c00d14a899a1" />

Below given image shows the NMOS transistor when Vgs = 0

<img width="1284" height="513" alt="image" src="https://github.com/user-attachments/assets/434eeee7-ce0b-41ef-a4ea-93358184a05f" />

Below given image shows the NMOS transistor when Vgs > Vth (threshold voltage):

<img width="1349" height="648" alt="image" src="https://github.com/user-attachments/assets/fcd4c65b-a43f-47a4-a56d-4afc874d5f1a" />

The equation to find the threshold voltage is given below

<img width="1328" height="674" alt="image" src="https://github.com/user-attachments/assets/20b742c5-ab6d-4d3e-9bfa-edd3b92530f5" />

---

## Resistive region of operation with small drain-source voltage
NMOS Resistive region of operation with small drain-source voltage
Resistive Region of Operation (VGS > Vt, small VDS)

<img width="1316" height="640" alt="image" src="https://github.com/user-attachments/assets/6cec18f4-ca87-45a9-b449-feca9289976f" />

At this stage:

- A strong inversion channel is established, allowing charge carriers to flow from the source to the drain.
- The gate-to-channel voltage at any point ‘x’ along the channel is given by V<sub>GS</sub> – V(x).
- The induced charge density (Q<sub>i</sub>) in the channel is directly proportional to (V<sub>GS</sub> – V<sub>T</sub>).
- The current flow is determined by the effective channel length (L) and the voltage variation V(x) along the channel.
- In this region, the MOSFET operates like a voltage-controlled resistor, where the channel resistance changes with gate voltage.

---

## Drift Current Theory (NMOS) — Overview

In the **resistive (linear) region** of an NMOS transistor — when **V<sub>GS</sub> > V<sub>T</sub>** and **V<sub>DS</sub>** is small — the drain current is dominated by **drift current**.

### Intuition

- Drift current is produced by the **electric field** across the channel when a small drain-to-source voltage is applied.  
- **Electrons** (majority carriers in an NMOS) are accelerated by this field and flow from source to drain.  
- In this region the transistor behaves like a **voltage-controlled resistor**: the effective channel resistance changes with the gate voltage.

### Key formulas

- Induced channel charge at position *x*:
\[
Q_i(x) = -C_{ox}\,\bigl[(V_{GS} - V(x)) - V_T\bigr]
\]

- Drain current (conceptual):
\[
I_D \;=\; (\text{carrier velocity}) \times (\text{available charge}) \times W
\]

Where:
- \(C_{ox}\) = gate oxide capacitance per unit area  
- \(V(x)\) = potential at position \(x\) along the channel  
- \(V_{GS}\) = gate-to-source voltage  
- \(V_T\) = threshold voltage  
- \(W\) = channel width  
- \(L\) = effective channel length (affects the voltage profile and resistance)

### Practical note

Because the local charge \(Q_i(x)\) depends on \(V(x)\), you can derive the classical linear-region MOSFET equation by integrating along the channel assuming drift-limited transport. For typical SPICE-level analysis, this behavior explains why the transistor looks like a resistor whose value depends on \(V_{GS}\).

<img width="1348" height="642" alt="image" src="https://github.com/user-attachments/assets/ea069442-f583-48b7-8eac-542a3923e5ca" />

<img width="418" height="430" alt="image" src="https://github.com/user-attachments/assets/0ac40d04-457c-45f3-906e-f5470af5429c" />

From the device point of view, we have two kinds of devices 
- Drift Current: Current due to potential difference.

<img width="1253" height="724" alt="image" src="https://github.com/user-attachments/assets/ce691427-9a2b-40b0-8f2c-03518503ec73" />

<img width="550" height="672" alt="image" src="https://github.com/user-attachments/assets/382fe185-72cb-432d-8766-a623c147988f" />

<img width="1267" height="534" alt="image" src="https://github.com/user-attachments/assets/93f01c19-0009-4be5-9942-9944d0d259cd" />

- Diffusion Current: Current due to difference in carrier concentration.

---

### Pinch-Off Region Condition

The images below illustrate how an NMOS transistor reaches the pinch-off condition when the voltage difference (V<sub>GS</sub> − V<sub>DS</sub>) ≤ V<sub>T</sub>.

At this point, the channel near the drain end disappears, as the gate can no longer maintain inversion in that region. This marks the transition from the linear (ohmic) region to the saturation region, where the current becomes almost independent of V<sub>DS</sub>

<img width="1286" height="657" alt="image" src="https://github.com/user-attachments/assets/fcd11a7a-0be3-44a4-854e-929207be990f" />

**Drain Current Model for Saturation Region of Operation**
These images show how the effective channel length reduces due to pinch-off and how the drain current (ID) becomes weakly dependent on VDS, leading to the saturation region equation with channel length modulation.

<img width="1303" height="648" alt="image" src="https://github.com/user-attachments/assets/75e713e4-9b6a-4cc6-83a2-c9947b644dd1" />

<img width="1206" height="662" alt="image" src="https://github.com/user-attachments/assets/1b89c58b-4390-4e29-b971-477f642745aa" />

---

## Basic SPICE Setup

Fabricating integrated circuits (ICs) is a costly and time-consuming process — even small design errors can result in expensive silicon re-spins. To avoid this, engineers use simulation tools to explore and validate circuit behavior before fabrication, reducing both development time and cost.

<img width="1227" height="647" alt="image" src="https://github.com/user-attachments/assets/ac64b1d9-d4aa-45bc-b3ef-4b6501a01473" />

<img width="1303" height="584" alt="image" src="https://github.com/user-attachments/assets/b584b884-eb85-4d20-b4d1-1730ebb4b0c0" />

### Simulation Levels in VLSI Design
Different types of simulators operate at various abstraction levels:

- **Process Simulators** (e.g., SUPREME): Model how fabrication steps affect device properties and physical characteristics.
- **Circuit Simulators** (e.g., SPICE, Spectre): Predict voltages, currents, power, and performance at the transistor level.
- **Logic Simulators** (e.g., VCS, ModelSim): Verify the functionality of digital logic described in HDL (Verilog/VHDL).
- **Architecture Simulators**: Evaluate high-level system behavior such as throughput and memory performance.

Among these, circuit and logic simulators are the most essential for VLSI designers.

### About SPICE

**SPICE (Simulation Program with Integrated Circuit Emphasis)**, originally developed at UC Berkeley, numerically solves nonlinear equations to simulate the behavior of electronic components like transistors, resistors, and capacitors.

Modern SPICE variants include both open-source tools like Ngspice and LTSpice, as well as commercial versions such as HSPICE and PSPICE.
SPICE Deck (Input File)
A SPICE simulator reads a text-based input file known as a SPICE deck, which typically contains:

- **Netlist**: List of circuit components and their connections
- **Device Models**: Physical and electrical parameters of transistors and other elements
- **Initial Conditions**: Starting voltages or currents
- **Inputs** (Stimuli): Excitation signals such as voltage or current sources
- **Simulation Commands**: Types of analysis (DC, AC, transient) and output options

The simulator then produces waveforms and reports, enabling designers to analyze and fine-tune their circuits before committing to silicon fabrication.

### SPICE Netlist

<img width="1166" height="541" alt="image" src="https://github.com/user-attachments/assets/521067c1-be79-4b8d-b7ef-ff05eb93d4cb" />

### Purpose

This circuit is designed to **bias an NMOS transistor** using **Vin** and **Vdd**, enabling SPICE simulation of its **I<sub>D</sub>–V<sub>DS</sub>** characteristics for a given **transistor geometry (W/L)** and resistive input.

### Technology Note

To simulate transistor behavior accurately, SPICE requires a **technology file** that defines the physical and electrical properties of NMOS and PMOS devices. Key parameters include:

- **Threshold voltage (V<sub>T</sub>)**  
- **Body effect coefficient (γ)**  
- **Oxide thickness (T<sub>OX</sub>)**  
- **Carrier mobility (μ<sub>0</sub>)**  
- **Other technology-specific constants**

These parameters allow SPICE to model the transistor’s response realistically under different biasing conditions.

<img width="942" height="656" alt="image" src="https://github.com/user-attachments/assets/eb6d049d-047b-4ccf-bfda-fbd335304b72" />

### Installation

#### Clone the workshop repository:
```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop
cd design
```
<img width="859" height="159" alt="image" src="https://github.com/user-attachments/assets/0b69ccfc-8baa-41db-99f7-740d8b9288c7" />

<img width="784" height="108" alt="image" src="https://github.com/user-attachments/assets/e1099eef-8403-475b-879e-d6a84ad04121" />

<img width="880" height="164" alt="image" src="https://github.com/user-attachments/assets/8b5c3792-8a6e-4d80-9a35-f1e6b36d5510" />


**Install ngspice (open-source SPICE simulator):**
```bash
sudo apt-get update
sudo apt-get install ngspice
```

#### Running Your First Simulation
**Execute a SPICE Netlist:**
```bash
ngspice day1_nfet_idvds_L2_W5.spice
```
**Generate Plots and for Saving Waveform Data**
```bash
plot -vdd#branch
write output.raw
wrdata output.csv -vdd
quit
less output.csv

```
<img width="877" height="680" alt="image" src="https://github.com/user-attachments/assets/a0aad573-45a8-4563-a1f7-f1d62cc4bf58" />


at desgin directory

```bash
vim day1_nfet_idvds_L2_W5.spice
```
<img width="1120" height="821" alt="image" src="https://github.com/user-attachments/assets/4434ebee-d1ea-4b7e-b970-a52aa20d34fa" />
