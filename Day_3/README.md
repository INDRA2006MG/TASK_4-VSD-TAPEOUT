
# CMOS Switching Thershold and Dynamic Simulation

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
