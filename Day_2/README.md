# Velocity Saturation and basics of CMOS inverter VTC

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
