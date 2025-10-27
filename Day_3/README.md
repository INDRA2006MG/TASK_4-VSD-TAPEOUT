
# CMOS Switching Threshold and Dynamic Simulation

## Introduction

The VTC is the plot of Vout (y-axis) vs Vin (x-axis) when Vin is slowly swept from 0 to VDD. It fully characterizes the static switching behavior of the inverter.
  - `Vout = fn(Vin)`

You gradually increase the input voltage (Vin) from 0 to VDD and record the corresponding steady-state output (Vout). The resulting VTC curve can be divided into three distinct regions:

Low input region (Vin ≈ 0) :
The PMOS transistor is fully ON, and the NMOS is OFF. As a result, the output voltage remains close to VDD.

Transition region: As Vin increases, both transistors conduct partially. The output voltage starts to drop from VDD toward 0 V. This is the region where the inverter switches states, and the switching threshold (Vm) is located here — the point where Vin = Vout.

High input region (Vin ≈ VDD):
The NMOS transistor is fully `ON`, the PMOS is `OFF`, and the output voltage settles near 0 V.

The S-shaped VTC curve appears because the NMOS and PMOS transistors take turns controlling the output as the input voltage changes.

**When Vin is low:**
The NMOS is completely `OFF`, and the PMOS is fully on. This creates a strong path from VDD to the output, keeping Vout `close to VDD`.

**As Vin starts to rise:**
The NMOS begins to turn `ON` while the PMOS gradually turns `OFF`. In this region, both transistors are partially conducting, and the output voltage depends on their relative strengths — similar to a voltage divider.

**Around the midpoint:**
Both transistors conduct about `equally`. This causes the output voltage to drop rapidly, creating the steep middle section of the curve. This is where the inverter switches state and exhibits high gain.

**When Vin is high:**
The PMOS is completely `OFF`, and the NMOS fully conducts, pulling the output down to ground (`0 V`).

---

## SPICE simulation for CMOS inverter

<img width="1304" height="599" alt="image" src="https://github.com/user-attachments/assets/145433fe-e20b-44be-8ad8-b52ab0ab535e" />

This image shows how to create a SPICE deck for a CMOS inverter.

To build it, you need to:

**Connect the components**: Define how the PMOS (M1), NMOS (M2), power supply (VDD), ground (VSS), input (Vin), and output (Vout) are connected in the circuit.

**Set component values**: Specify details like transistor sizes (W/L), supply voltage (e.g., 2.5V), and the load capacitor (e.g., Cload = 10 fF).

**Identify circuit nodes**: Understand the key nodes in the circuit — for example, `in`, `out`, `VDD`, `VSS`, and the terminals of each transistor.

**Assign names to the nodes**: Give each node a clear and consistent name to make the netlist easier to write, simulate, and interpret.

By doing this, you create a clean and accurate SPICE netlist that can be used to simulate the CMOS inverter correctly.


```bash
vim day3_inv_vtc_Wp084_Wn036.spice
```
<img width="997" height="791" alt="image" src="https://github.com/user-attachments/assets/aa909098-7342-4e78-a8ae-6f99abd43101" />

to plot the graph 
```bash
ngspice day3_inv_vtc_Wp084_Wn036.spice
plot out vs in
```

<img width="876" height="681" alt="image" src="https://github.com/user-attachments/assets/5ed029ea-a026-404f-8181-dd4f791b6d8e" />

```bash
vim day3_inv_tran_Wp084_Wn036.spice 
```
<img width="1086" height="727" alt="image" src="https://github.com/user-attachments/assets/7538e97c-fd95-4e2e-b950-cf4afbe7cbb7" />

```bash
ngspice day3_inv_tran_Wp084_Wn036.spice 
plot out vs time in
exit
```
<img width="883" height="678" alt="image" src="https://github.com/user-attachments/assets/9f5cd2f8-373c-4a67-899b-616aa3367eec" />
### Switching Threshold, Vm

By comparing both the graphs, we can able to study about the switching threshold 

<img width="1210" height="650" alt="image" src="https://github.com/user-attachments/assets/2fa303fc-bcd4-4e72-a870-8abb7f131b72" />

- If the Vin is high then the Vout is low and vice versa.
- The switching threshold voltage (Vm) is the point on the VTC (Voltage Transfer Characteristic) curve where the input voltage equals the output voltage:  
    - Vin = Vout​
- This is a crucial parameter because it directly affects the noise margins and reliability of the inverter.

At this voltage:

- Both NMOS and PMOS transistors are ON.
- They operate in the saturation region.
- This leads to high leakage current.  This gives the direct path to the VDD to VSS.
- The inverter exhibits high voltage gain, resulting in a steep transition in the VTC curve.

**Left graph**:    
     - Resulting `Vm ≈ 0.98 V`

**Right graph**:  
     - Resulting `Vm ≈ 1.2 V`

**Regions of operation:**

- Different regions of the curve correspond to the transistor operating regions:
  - **PMOS Linear / NMOS OFF**
  - **PMOS Linear / NMOS Saturation**
  - **PMOS Saturation / NMOS Saturation** — This is where `Vm` is located.
  - **PMOS Saturation / NMOS Linear**
  - **PMOS OFF / NMOS Linear**

<img width="1111" height="568" alt="image" src="https://github.com/user-attachments/assets/0c3ed069-c29c-494f-8c93-e33b4d49b366" />

The value of drift current of both CMOS transistor can be given by the following equations shown in the below figure,

<img width="978" height="559" alt="image" src="https://github.com/user-attachments/assets/88f35459-4f48-49d5-bf2f-31dac54ca5d3" />

Now, equating Idsp + Idsn = 0, we get the following equation

<img width="852" height="425" alt="image" src="https://github.com/user-attachments/assets/a8873d8e-fb56-452f-85dd-9ea92c1dd137" />

and also we can able to set the value of `Vm` and we can able to find the ration `W/L`.

<img width="963" height="224" alt="image" src="https://github.com/user-attachments/assets/a83f2e40-391d-4539-94e9-acf5d5fc8619" />

From the table given below, we observe that,
- When Wp/Lp ≈ 2 × Wn/Ln, the inverter achieves balanced rise and fall delays (≈ 80 ps each).
- At this point, the switching threshold Vm ≈ 1.2 V.

<img width="719" height="255" alt="image" src="https://github.com/user-attachments/assets/5b54f34b-a305-493d-85ac-a97a039dd71c" />

---

## Key Takeaways from PMOS & NMOS Sizing

### Optimal Clock Inverter Design
- **Condition:** `(Wp/Lp) = 2 × (Wn/Ln)`
- **Why it works:** Produces nearly equal rise and fall delays.
- **Use case:** Ideal for clock buffers and clock tree design, where symmetrical timing is important.

---

### Data Path Inverters
- Other PMOS/NMOS ratios can be used as standard inverters or buffers.
- Preferred for data path applications, not necessarily for clock networks.

---

### Switching Threshold Characteristics
- For PMOS/NMOS ratios of:
  - `(Wp/Lp) = 2 × (Wn/Ln)` and `(Wp/Lp) = 3 × (Wn/Ln)` → Switching threshold voltage is very low.
  - `(Wp/Lp) = 4 × (Wn/Ln)` and `(Wp/Lp) = 5 × (Wn/Ln)` → Also results in a very low switching threshold.
- **Implication:** The inverter switches earlier as input rises.

---

### Impact of PMOS Width
- **Increasing Wp/Lp:** Significantly reduces rise delay.
- **Reason:** A larger PMOS can supply more current to charge the output capacitance faster.
- **Result:** Faster rise time of the output voltage.

---

### On-Resistance Relationship
- R_on(PMOS) ≈ 2.5 × R_on(NMOS)
- PMOS has higher on-resistance, so it is made wider to balance rise/fall characteristics.

---

### CMOS Inverter Applications in Chip Design

#### Static Timing Analysis (STA)
- Used as reference delay cells for modeling timing arcs.
- Helps calculate rise/fall delays, setup, and hold times.

#### Clock Tree Synthesis (CTS)
- Serve as clock buffers for driving fanout.
- Regenerate clock signals and introduce intentional skew to balance clock network delays across the chip.
- By replacing clock invertor cell instead of noraml invertor cells.

If the rise and fall delays of a clock buffer are closely matched, no duty cycle correction is required.

<img width="1294" height="726" alt="image" src="https://github.com/user-attachments/assets/cf17c3ce-04af-478d-bb44-56590df65806" />

However, when there is an imbalance—often caused by a mismatch between PMOS and NMOS on-resistances—duty cycle correction circuits are employed in the clock tree to restore and maintain a 50% duty cycle.
