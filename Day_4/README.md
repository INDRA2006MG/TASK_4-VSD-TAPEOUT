# Static Behavior Evaluation: CMOS Inverter Robustness and Noise Margin

## Introduction to noise margin

Noise margin is the maximum noise voltage a CMOS circuit can tolerate without logic errors.

<img width="786" height="534" alt="image" src="https://github.com/user-attachments/assets/69258608-a965-4f24-a4b9-7b4307e378e4" />

### CMOS Inverter Characteristics and Noise Margin

This figure compares:

- **Ideal Inverter I/O:** Shows an abrupt switching at **V<sub>DD</sub>/2** with an infinite slope (left side).  
- **Actual Inverter I/O:** Exhibits a finite slope with a **gradual transition region** (right side).

<img width="781" height="492" alt="image" src="https://github.com/user-attachments/assets/7b072f9a-a1b5-4891-b532-7234c424e24e" />

#### Noise Margin Definition

The figure also illustrates how **Noise Margin** is derived from the **Voltage Transfer Characteristic (VTC)** of a CMOS inverter.  
The **undefined region** in the VTC corresponds to the input voltage range where the output is not clearly defined as logic '0' or '1'. Noise margins quantify the inverter’s tolerance to input noise within this region.

### Voltage Transfer Characteristic (VTC) and Noise Margins

#### Left Plot Highlights:

- The slope of the VTC equals −1 at two critical points:
     - V<sub>IL</sub>: Input Low Threshold Voltage
     - V<sub>IH</sub>: Input High Threshold Voltage
- Right Plot Highlights:
     - V<sub>OH</sub> and V<sub>OL</sub>: Valid output high and low voltage levels
     - V<sub>IL</sub> and V<sub>IH</sub>: Input thresholds where the slope = −1
- Noise Margins:
     - NMH = V<sub>OH</sub> − V<sub>IH</sub> → Maximum noise tolerance for a logic ‘1’
     - NML = V<sub>IL</sub> − V<sub>OL</sub> → Maximum noise tolerance for a logic ‘0’
- Undefined Region:
     - Between V<sub>IL</sub> and V<sub>IH</sub>, the logic level is undefined.
     - Any noise within this range can produce unstable or invalid outputs.
- Design Goal:
     - Maximize NMH and NML to improve circuit robustness.
     - Noise margins indicate how much a logic signal can tolerate noise before causing errors, ensuring reliable operation in noisy environments.

<img width="744" height="484" alt="image" src="https://github.com/user-attachments/assets/100bdb99-43cc-4f3d-bdf3-125a4dda0461" />

<img width="766" height="467" alt="image" src="https://github.com/user-attachments/assets/8cb4cab9-2746-4f73-b609-c5945a098bf3" />

## Noise Margin Behavior Analysis

**Safe Glitch:** If the noise spike stays between V<sub>OL</sub> and V<sub>IL</sub>, it is still interpreted as a valid logic ‘0’.
**Potentially Hazardous Glitch**: If the noise spike falls within the undefined region (between V<sub>IL</sub> and V<sub>IH</sub>), the logic level becomes uncertain — it may be read as either ‘0’ or ‘1’.
**Critical Glitch:** If the noise exceeds V<sub>IH</sub> but remains below V<sub>OH</sub>, it will be interpreted as logic ‘1’, which can cause errors and must be corrected.

This analysis helps designers understand how noise interacts with voltage thresholds and ensures circuit robustness.

<img width="1274" height="613" alt="image" src="https://github.com/user-attachments/assets/3c45eb54-4f8b-4c5c-9653-c7929b42dca7" />

### Key Observations on CMOS Inverter Noise Margins

When (W<sub>p</sub>/L<sub>p</sub>) = 2 × (W<sub>n</sub>/L<sub>n</sub>):
- The high-level noise margin (NMH) increases.
- Reason: The PMOS transistor is stronger in this configuration, allowing it to better maintain the output voltage at logic ‘1’ by effectively holding charges on the load capacitance.

When (W<sub>p</sub>/L<sub>p</sub>) = 4 × (W<sub>n</sub>/L<sub>n</sub>):
- The low-level noise margin (NML) drops slightly.
- Reason: The NMOS transistor becomes weaker relative to the PMOS, making it less effective at pulling the output fully down to logic ‘0’.

When (W<sub>p</sub>/L<sub>p</sub>) = 5 × (W<sub>n</sub>/L<sub>n</sub>):
- The NMH reaches near a steady value, showing that further increasing the PMOS width does not significantly improve the high-level noise margin.

**Overall Observation:+**

- The low-level noise margin (NML) remains fairly stable across these configurations.
- The high-level noise margin (NMH) increases by approximately 120 mV as the PMOS is strengthened, highlighting the robustness of the CMOS inverter against input noise at logic ‘1’.

---

```bash
vim day4_inv_noisemargin_wp1_wn036.spice 
```
<img width="1196" height="840" alt="image" src="https://github.com/user-attachments/assets/07f1e4b5-ba67-49fc-90a9-e23da93d9287" />

```bash
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot out vs in
```
<img width="870" height="683" alt="image" src="https://github.com/user-attachments/assets/6e6e5158-383d-4635-b147-7c357d722ec7" />
