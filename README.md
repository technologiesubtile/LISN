# LISN
Line Impedance Stabilization Network

A LISN is a box that presents a well-defined source impedance to an electrical consumer in order to measure the emissions that it re-injects into the power grid, as part of electromagnetic compliance (EMC) testing.

![eevblog_cispr_lisn_schematics](https://github.com/user-attachments/assets/2d3cc524-24a0-46f3-85c0-db89c3493d5f)


Here is the schematics of a LISN that is quite standard, it is sold as Tekbox and the schematics and even the gerber files of the layout are published on the manufacturers website. It is a 5 µH, 50 Ohm version for voltage < 200 V and current < 10 A. This can be used to check compliance with standards such as CISPR25 for automotive applications. It furthermore requires a spectrum analyzer to be connected to its coax output and to measure the spectrum from 150 kHz to 100 MHz, and to make sure it stays below the tolerable curve.

![P3151803](https://github.com/user-attachments/assets/8381b36a-c79b-439b-8ef4-f32638255485)

I redeveloped the layout for a circuit identical to the Tekbox LISN to implement it with through-hole components. 

Commercial units can be quite costly especially the ones for 230 VAC line voltage that usually feature an isolation transformer.

Now we do some measurements of its output impedance with the NanoVNA. To give an idea what it should look like, here the curves for the upper and lower limits, copied screenshot from EEVblog, and in the middle, a LISN used by Dave in his youtube video. 

![eevblog_cispr_lisn_nominal](https://github.com/user-attachments/assets/fa9db483-89ff-4db8-ba68-fd3dc2675d40)


So we understand that basically the idealized replacement circuit is a parallel circuit of 5 uH and 50 Ohm.

It turns out that the cabling inside the box is too long and the output impedance of our LISN rises to > 200 Ohm at 100 MHz. Before showing curves, let's trim the leads to the minimum.

![P3201822](https://github.com/user-attachments/assets/69fa2651-419a-474a-9d4e-eecc8ede6a72)  ![P3201823](https://github.com/user-attachments/assets/8111f4a1-ea33-4821-b9ad-254df51b7dd6)

The measurement setup: we perform S11 reflection measurements on the 4 mm connector. The input port 2 of the VNA is connected to the coax output for the sole reason of terminating it, since the 50 Ohm load connected to the LISN output is part of what determines its input impedance.

![P3201836](https://github.com/user-attachments/assets/384b5663-d0a5-4ada-a9ab-91bb98bb1f66)

Now let's acquire some curves in the range 1 MHz to 100 MHz:

![P3201828](https://github.com/user-attachments/assets/79e72d77-c586-4cc9-ab81-840ce5ab3410)  ![P3201844](https://github.com/user-attachments/assets/8266cff3-123a-41cd-a94b-ef1c064ddf58)
![P3201826](https://github.com/user-attachments/assets/b53afc89-86ca-4f1d-be5c-de4b3f909943)  ![P3201835](https://github.com/user-attachments/assets/cabad535-b50a-47a4-8927-bba2acd8b6df)

Here the S11 parameter along with component values. But note that these values are the ones of a series circuit. Since we have a parallel circuit, we can only look at one of the values while the other is a negligibly high impedance.

The straight onset corresponds to only 2 measurement points, 100 kHz and 1.099 MHz so nothing shall be conluded from it. The first curve with the cursor at 1.099 MHz and a phase of like 45 degree correspond to a case where the impedance of the coil shall be around i*35 Ohm and the parallel resistive load is 50 Ohm which should result in something like 20 Ohm total. It shows around 15 Ohm.

The second cursor reading about 9 MHz, and 35 Ohm. Now, the coil impedance becomes negligibly high and the impedance approaches 50 Ohm (roughly..) and the phase zero.

Third position, 30 MHz, 47 Ohm, not bad. Forth position, 100 MHz, 112 Ohm. Here again, we are a bit out of the bounds of the CISPR-25 specifications. 

However, we can safely attribute this to the remaining length of the non-coaxial wiring inside the box. It should not bother us too much, taking into account that for the test setup, the EUT will be connected to the 4 mm clamps with a piece of non-coaxial wire of a length between 20 cm and 30 cm. So the ~ 8 cm length of cabling within the LISN should not deteriorate the result more than already does the outside wiring.

So far we have established that the impedance is close enough to 50 Ohm between 10 MHz and 30 MHz, but we have not measured the inductance yet. For doing so, we shall focus on lower frequency range where the parallel LR circuit is dominated by the inductance. But for this omega.L should be below 5 Ohm and hence f < 300 kHz. However, this coincides with the impedance range where S11 reflection measurements become inacurate. Let's try anyway from 0 to 3 MHz.


![P3201842](https://github.com/user-attachments/assets/6fc4d233-3d56-460a-bc73-1ae1cd67fbf1)  ![P3201837](https://github.com/user-attachments/assets/7046e02e-1e45-47d9-bc4d-0f9b6758e125)
![P3201839](https://github.com/user-attachments/assets/39417820-4aa4-4bbf-b5da-eebccb645d23)

At 70 kHz, we measure even a negative inductance (= capacitanc), but at 640 kHz around 3.5 uH and at 2.2 MHz again 1.something uH. The conclusion is, at 640 kHz we are close to the 5 uH, but at lower frequency the inductive reactance becomes too small to be measured in S11 reflectance and at 2.2 Mhz again the 50 Ohm real resistive load is dominant.

To do better, we should do a 2-port shunt-through measurement. For this we first have to change the cabling, connect both coaxial cables in parallel to the 4 mm clamps, and then we must export the S21 data from the VNA and make a spreadsheet that converts it to component values. The only conversion that the nanoVNA can do natively is from S11 into a RLC series circuit.

For the moment we shall assume that our LISN is close enough to the specifications. Surely it would not get accredited, but for some in-house precompliance testing it is good enough.
