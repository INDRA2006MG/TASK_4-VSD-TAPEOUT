# Velocity Saturation and basics of CMOS inverter VTC

From the graph, we can able to observe that it has two different areas which is identified using the Vds = Vgs - Vt. 

**Linear region:**
- The drain current is linear function of the drain-source voltage (when drain-source voltage is not exceed Vds)
- It is defined for Vds < (Vgs -Vt)

**Saturation region:**
- The drain current (Id) depends on channel length modulation and Vds.
- It is defined for Vds ≥ (Vgs - Vt).

- The region before Vds = Vgs - Vt is the Linear Region, where Id varies linearly with Vds.
- The region after Vds = Vgs - Vt is the Saturation Region, where Id is influenced by channel length modulation and Vds.

<img width="1162" height="719" alt="image" src="https://github.com/user-attachments/assets/d3ff32c8-789c-4ea2-b72f-e2934a87b4f7" />

**Observation 1: Long Channel Vs Short Channel for NMOS**
The plot below compares NMOS output characteristics for long channel and short channel devices with same W/L ratio:

<img width="1376" height="582" alt="image" src="https://github.com/user-attachments/assets/1603b648-9bd3-48c5-985c-887d8b6c33f7" />

In the figure, the left plot shows a long-channel NMOS transistor with W = 1.8 μm and L = 1.2 μm, while the right plot shows a short-channel transistor sized at W = 0.375 μm and L = 0.25 μm.

Since the channel length of the second device is below 0.25 μm, it falls into the short-channel MOSFET category.

Even though both transistors have the **same W/L ratio**, their actual width and length values are different. This setup helps us fairly compare how device behavior changes when the channel is scaled down, without letting the W/L ratio influence the results.

**Long Channel**
- There is a quadratic dependencies on gate voltage.
- In this, the drain current is in quadratic function of the gate voltage always.
- In long-channel devices, carriers accelerate freely, giving higher Id.

**Short Channel**
- There is a linear dependence on gate voltage.
- One of the short channel effect is the Voltage Saturation Effect.
- In the short-channel device, the drain current is quadratic with gate voltage at lower Vgs values, but at higher Vgs, it becomes almost linear.

When Vds is fixed and we gradually increase Vgs:

- For long-channel MOSFETs, the drain current (Id) increases in a smooth, quadratic way as Vgs rises. This matches the ideal MOSFET behavior described in basic theory.
- For short-channel MOSFETs, Id initially follows the same quadratic pattern at lower values of Vgs. But as Vgs continues to increase, the curve starts to level off and becomes more linear instead of quadratic.

This happens because of velocity saturation. At high electric fields, the electrons in the channel reach their maximum speed. Even if Vgs increases further, the electrons can’t move any faster, so the current stops following the ideal quadratic law and grows more slowly — almost linearly.

So, the plot clearly shows how in short-channel devices, velocity saturation changes the Id–Vgs behavior, making it shift from quadratic to linear at higher gate voltages.


For Short Channels, there are totally four regions.
So first of all, we gonna see the voltage saturation effect,

- At the lower electric fields, velocity is the linear function of the electric fields
- After some point, at the higher electric fields, it is saturated that's the velocity becomes constant due to *Scattering Effect*
  
<img width="1387" height="595" alt="image" src="https://github.com/user-attachments/assets/58ab16db-c7b9-4bcb-ae47-fdc0a045c701" />

for continuity, we put E=Ec and we get the value of critical electric field value is as follows
<img width="1025" height="606" alt="image" src="https://github.com/user-attachments/assets/925edd97-2b2a-401f-b709-38da88266ea3" />

After substitute the value of Vn to drain current, we got a complex equation

<img width="1238" height="369" alt="image" src="https://github.com/user-attachments/assets/0fd6237a-7e0e-456b-98f3-53313ae58544" />

Now let's dive into Voltage Saturation Effect,

<img width="986" height="496" alt="image" src="https://github.com/user-attachments/assets/9101f0bc-b9b2-4e6c-8dd9-4d3ff2290c1b" />

Let's analyse that when the value of Vgt is minimum, the transistor will be in the saturation region.
For Saturation region which is higher voltage for Vds operation, the value of drain current is
<img width="455" height="195" alt="image" src="https://github.com/user-attachments/assets/38ce5e0c-ec4f-42b4-984a-b6cd20701744" />

Now for the value of Vds is minimum, then the transistor is in resistive or linear region of operation.  
<img width="422" height="92" alt="image" src="https://github.com/user-attachments/assets/8da164f9-01df-4ebc-8e3c-dcaa78573d1f" />
then the third term which has [1+λ Vds] will be approx. equal to one.

Now in the short channel, when the Vsat is minimum
<img width="932" height="131" alt="image" src="https://github.com/user-attachments/assets/de20c8bf-6b9d-4181-94fc-e1558529659a" /># Velocity Saturation and basics of CMOS inverter VTC

```bash
vim day2_nfet_idvds_L015_W039.spice
```

<img width="1152" height="785" alt="image" src="https://github.com/user-attachments/assets/2f830d16-b06f-4983-b632-ca9907ec486f" />

```bash
ngspice day2_nfet_idvds_L015_W039.spice
plot -vdd#branch
exit
```
<img width="876" height="673" alt="image" src="https://github.com/user-attachments/assets/de9f301b-a1aa-4a6e-8ee2-84c8155ea47b" />

### Now you can able to see the graph for vds
<img width="867" height="684" alt="image" src="https://github.com/user-attachments/assets/bd81d7a6-c58e-420f-b5a0-0b9d0a632e1a" />

```bash
ngspice day2_nfet_idvgs_L015_W039.spice
plot -vdd#branch
```
### Now you can able to see the graph for vgs 
<img width="691" height="545" alt="image" src="https://github.com/user-attachments/assets/5479ec37-c25f-45ac-9081-bef5a3981f40" />
## CMOS Voltage-Transfer Characteristics

- It defines the delays of the cells
- This helps us to derive the delay for a perticular cell.

**MOSFET as a Switch:**
NMOS: + Vgs and + Vt
PMOS: - Vgs and - Vt
 - OFF State:
   - The MOSFET behaves as an open switch (infinite OFF resistance) when:
   - |Vgs| < |Vt|

 - ON State:
   - The MOSFET behaves as a closed switch (finite ON resistance) when:
   - |Vgs| > |Vt|

<img width="903" height="521" alt="image" src="https://github.com/user-attachments/assets/36a25016-81da-4700-be61-7daf19a6eb74" />

---

### Introduction to standard MOS voltage current parameters
 
<img width="1273" height="464" alt="image" src="https://github.com/user-attachments/assets/a3e356cd-b0e9-47ff-a885-689317693c4e" />

- The left diagram shows a CMOS inverter at the transistor level: the PMOS transistor is connected to Vdd, the NMOS transistor is connected to Vss, and Vin is applied to both gates. The output Vout is taken from the common drain node, with CL representing the load capacitance.

- The middle diagram illustrates the switch model when Vin = Vdd: the NMOS transistor is ON (acting as a resistor Rn), while the PMOS is OFF (open switch), resulting in Vout = 0.
     - When Vin = Vdd → Vout = 0 (NMOS ON, PMOS OFF).
     - This is due to when the capacitor fully charges and then a direct path exists from capacitor to the NMOS due to the discharging of capacitor(CL).

- The right diagram shows the switch model when Vin = 0: the PMOS transistor is ON (acting as a resistor Rp), while the NMOS is OFF (open switch), producing Vout = Vdd.
     - When Vin = 0 → Vout = Vdd (PMOS ON, NMOS OFF).
     - Due to the charging effect made by the PMOS, the Vout = Vdd.

---

### Load Line Curves for NMOS and PMOS

Convert the PMOS gate-source voltage (VgsP) into an equivalent Vin and plot to the load voltage transfer characteristic.
By this, we can able to identifies the delays.
Replace all internal node voltages with Vin, Vdd, Vss, and Vout.

<img width="1395" height="707" alt="image" src="https://github.com/user-attachments/assets/e726712f-7cc2-4448-81df-ee2ecadb4fce" />

Convert PMOS and NMOS drain-source voltages to Vout.

- Obtain the Load Curve for PMOS Transistor
  
<img width="1382" height="729" alt="image" src="https://github.com/user-attachments/assets/881f60e0-cf3b-4d40-8c1c-938e67989123" />

From the load curve, we can able to observe that the capacitor is completely discharged so that Vout = 0.

- Obtain the Load Curve for NMOS Transistor

<img width="970" height="734" alt="image" src="https://github.com/user-attachments/assets/c7b85db2-6c75-46c9-b1ce-2ae698b122d6" />

### Merge PMOS – NMOS load curves and plot VTC

<img width="1376" height="733" alt="image" src="https://github.com/user-attachments/assets/0344e3d9-87bd-4ed8-a998-dce112f1f2d5" />
