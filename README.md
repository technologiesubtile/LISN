# LISN
Line Impedance Stabilization Network

A LISN is a box that presents a well-defined source impedance to an electrical consumer in order to measure the emissions that it re-injects into the power grid, as part of electromagnetic compliance (EMC) testing.

![P3151803](https://github.com/user-attachments/assets/8381b36a-c79b-439b-8ef4-f32638255485)

I redeveloped the layout for a circuit identical to the Tekbox LISN to implement it with through-hole components. It is a 5 µH, 50 Ohm version for voltage < 200 V and current < 10 A. This can be used to check compliance with standards such as CISPR25 for automotive applications. It furthermore requires a spectrum analyzer to be connected to its coax output and to measure the spectrum from 150 kHz to 200 MHz, and to make sure it stays below the tolerable curve.

Commercial units can be quite costly especially the ones for 230 VAC line voltage that usually feature an isolation transformer.
