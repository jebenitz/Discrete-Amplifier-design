# Design workflow 
## Stage 1.0: Differential Amplifier with Resistive Load

Differential pair is biased at I_EE = 2mA through a current mirror with an I_ref set at the same value, so in common-mode, I_C = 1mA. With a V_T = 26mV, the transconductance is defined as g_m= I_C / V_T = 0.0384 [A/V].

To provide a minimum gain of 50, R_C_min = A_d / g_m = 1.3 K [Ω]

Transistors will remain in active region as long as V_C > V_B. V_B is approximately 0.7 V, with a tolerance of 1 V this yields V_C_min = 1.7 V.

Thus R_C_max = (V_CC – V_C_min) / I_C ≈ 10 K [Ω]

The appropriate range for R_C is between 1.3 K and 10 K.

Considering CMIR to determine the most convenient value of R_C, a parametrized DC sweep was done using a V_CM DC voltage source with the following directives:
```
.dc V4 -12 12
.step param Rc list 4.7k 5.1k 5.6k 8.2k 10k
```

Around 5 kΩ the CMIR range is widest, so the commercial value 5.1 kΩ was chosen.

For reference:
- V_CM_max = V_CC – I_C×R_C + 0.4 ≈ 7.3 [V]
- V_CM_min = V_EE + V_E + V_BE ≈ -10.8 [V]

The operating point is set at V_out = V_CC – I_C×R_C = 6.9 [V]. Executing .op directive gives a similar result (simulated value: 6.4 V, slightly lower due to base currents).

The expected differential gain is A_d = g_m×R_C ≈ 196 (simulated: 200).

Output resistance for differential output (Vout+ - Vout-) is approximately R_out ≈ 2×R_C = 10.2 K [Ω] (simulated: 9.69 kΩ, due to effects of V_A).

Input resistance in differential mode is R_id = 2×r_π = 2×β×V_T / I_C. From .op, β ≈ 200, giving R_id ≈ 10.4 K [Ω] (simulated: 10.05 kΩ).

The following simulation directives were used for characterization:
```
.op
.ac dec 10 1 100meg
.tf V(Vout+,Vout-) V1
```

Additional measurements:
- Differential input resistance: `.meas OP R_id PARAM {2*0.0258/Ib(Q1)}`
- Total current consumption: `.meas OP I_t PARAM {abs(I(V5)) + abs(I(V3))}` (result: 8.35 mA)
- Bandwidth: measured at frequency where gain drops by 3 dB from low-frequency value (result: 6.02 MHz)


