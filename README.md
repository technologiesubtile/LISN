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

Here the S11 parameter along with component values. But note that the component values displayed by the NanoVNA are the ones of a series circuit. Since we have a parallel circuit, we can only look at one of the values while the other is a negligibly high impedance.

The straight onset corresponds to only 2 measurement points, 100 kHz and 1.099 MHz so nothing shall be conluded from it. The first curve with the cursor at 1.099 MHz and a phase of like 135 degree corresponds to a case where the impedance of the coil shall be around i*35 Ohm and the parallel resistive load is 50 Ohm which should result in something like 20 Ohm total. It shows around 15 Ohm.

The second cursor reading about 9 MHz, and 35 Ohm. Now, the coil impedance becomes negligibly high and the impedance approaches 50 Ohm (roughly..) and the phase zero.

Third position, 30 MHz, 47 Ohm, not bad. Forth position, 100 MHz, 112 Ohm. Here again, we are a bit out of the bounds of the CISPR-25 specifications. 

However, we can safely attribute this to the remaining length of the non-coaxial wiring inside the box. It should not bother us too much, taking into account that for the test setup, the EUT will be connected to the 4 mm clamps with a piece of non-coaxial wire of a length between 20 cm and 30 cm. So the ~ 8 cm length of cabling within the LISN should not deteriorate the result more than already does the outside wiring.

So far we have established that the impedance is close enough to 50 Ohm between 10 MHz and 30 MHz, but we have not measured the inductance yet. For doing so, we shall focus on lower frequency range where the parallel LR circuit is dominated by the inductance. But for this omega.L should be below 5 Ohm and hence f < 300 kHz. However, this coincides with the impedance range where S11 reflection measurements become inaccurate. Let's try anyway from 0 to 3 MHz.


![P3201842](https://github.com/user-attachments/assets/6fc4d233-3d56-460a-bc73-1ae1cd67fbf1)  ![P3201837](https://github.com/user-attachments/assets/7046e02e-1e45-47d9-bc4d-0f9b6758e125)
![P3201839](https://github.com/user-attachments/assets/39417820-4aa4-4bbf-b5da-eebccb645d23)

At 70 kHz, we measure even a negative inductance (= capacitanc), but at 640 kHz around 3.5 uH and at 2.2 MHz again 1.something uH. The conclusion is, at 640 kHz we are close to the 5 uH, but at lower frequency the inductive reactance becomes too small to be measured in S11 reflectance and at 2.2 Mhz again the 50 Ohm real resistive load is dominant.

To do better, we perform a 2-port shunt-through measurement. For this we first have to change the cabling and connect both coaxial cables in parallel to the 4 mm clamps.

![P3221845](https://github.com/user-attachments/assets/f25a4415-04a5-43b9-ab56-7e32835dbdcd)  ![P3221846](https://github.com/user-attachments/assets/56fc96b6-22d4-46f6-b767-765ec2d98639)

Then we measure the two ranges, from 100 kHz to 3 MHz, and from 1 MHz to 100 MHz. This time we use the nanoVNA saver software.

![S21Zshunt_100kto3M](https://github.com/user-attachments/assets/d3dfdc59-cc76-4129-aa59-bbc13261e1f8)  ![S21Zshunt_1Mto100M](https://github.com/user-attachments/assets/d6bed62b-a2a9-4295-afb0-4bcd54d68b52)

For the lower frequency range, it looks quite close to the expected value, at 300 kHz and Z = 7.5 Ohm we find L= 3.75 uH if we were to neglect the 50 Ohm real impedance in parallel. In the higher frequency range, the impedance still exceeds the CISPR25 boundary, 62 Ohm instead 57 Ohm, but much less than what we found with the single port measurement. We also find a little resonance peak at 34 MHz. 

The software in contrast to the nanoVNA device can already convert the S21 of the two-port measurement to Z_shunt and further to a R + iX series circuit of the shunt. The nanoVNA itself can only derive component values from single port S11 measurements and also only into a series circuit. To decompose our Z_shunt into a parallel RL circuit, we would have to export the S21 data in touchstone format and make a spreadsheet that converts it to component values.

We do so, and thereby realize that the calibration done in nanoVNA saver is no longer valid, because it transfers uncalibrated touchstone data and does the calibration in the soft. So we redo the measurements, for each range with its own device calibration, then import the touchstone data in the spreadsheet, and also concatenate the two ranges. The result :

![lisn5uH50ohm_impedance_shuntthrough](https://github.com/user-attachments/assets/44dc48a3-0063-4242-8b62-b54dda2bb628)

Note that we also use semilog axes here as in the reference from EEVblog, which would also have been possible in the nanoVNA saver software. Now we find even better values, L = 4.83 uH, and the impedance only rises to 61 Ohm at 100 MHz.

In conclusion, our LISN is quite close to the specifications since it only slightly excedes the impedance boundary at 100 MHz. At low frequency the inductance is close to the reference of 5 uH, and Z @ 1 MHz is 23 Ohm and is right in the middle between the boundaries. For all practical means this LISN should be good enough.

Now we test a USB car charger. The setup is the car charger plugged into a cigarette lighter socket and connected to the LISN output. Attached to the USB charger is a power delivery trigger board that negotiates 12 V output, and a 25 W automotive light bulb (both filaments in parallel) as a load. On the right, the TinySA Ultra ZN-407 connected to the RF output of the LISN via a 20 dB attenuator that is taken into account by the setting "external gain" in the TinySA.

![P3301856](https://github.com/user-attachments/assets/14b0b30d-408d-4514-85f1-5b6c8f2e1ebe)  ![P3301855](https://github.com/user-attachments/assets/1b77e3b1-e0da-41b8-a32d-4b57c3f4872b)

The LISN is powered by a lab power supply set to 14.8 V. The USB car charger measures 14.2 V :

![P3301859](https://github.com/user-attachments/assets/766450e3-15d1-432a-996f-fb644abf65d2)   ![P3301858](https://github.com/user-attachments/assets/24f7163a-29e1-405d-b924-b4765fa1a93e)

The spectra are acqired in two ranges, first from 150 kHz to 4.19 MHz in 450 steps of 9 kHz and with a 9 kHz RBW as specified by the CISPR25 standard. Then, from 4 MHz to 108 MHz in steps of 230 kHz but with the RBW still set to 9 kHz, not respecting the standard. The spectra are then stitched with the transition at 4 MHz.

![car_usb_cispr25_sm](https://github.com/user-attachments/assets/3106cafb-dbe1-4805-b6cc-f4ec154a514e)

In red, the spectrum of the car charger operating from the lab power supply as shown before. At the 4 MHz transition, it seems that the TinySA adapts the RBW to the step size irrespective of its setting, and thereby the remainder of the curve seems to follow the envelope of the low frequency part. In black, the background with the lab power supply left in place. In green, background with battery power instead of power supply. In cyan, the car charger operating from battery. It is noteworthy that under battery power, at the transition of 4 MHz, the curve does not seem to follow the envelope as it did before. We have observed a sudden drop of the noise during the measurement under battery power. We suspect that during the acquisition with battery power, the battery voltage dropped and the mode of operation switched from switching mode to continuous conduction and therefore the noise decreased.

It is also noteworthy that when the TinySA is set to the units dB µV, it is still necessary to apply a conversion of 20*log() to the touchstone files to obtain the same reading as on the TinySA display. Also, the NanoVNA saver software used to obtain the touchstone files does not seem to operate the TinySA device correctly. When presssing the sweep button, it restarts a sweep on the SA, but it saves the data as on the screen from the previous, possibly incomplete sweep. So it can only instantaneously read the onscreen data but not start a sweep, wait until it is done and then save it.

It is understood that the USB charger would fail the CISPR25 test with its limit probably at 73 dB µV, but also that even the background spectra almost reach this value at low frequency. It is not the USB power of the TinySA that causes this because even when the TinySA is running on battery, it is that high. Probably it is just not made for such low frequency.

What can also be concluded from the spectra is the switching frequency of the car charger. The first peak is at 250 kHz, the second at 375 kHz and the third one is at 500 kHz, and therefore, the switching frequency is equal to the spacing of 125 kHz, and the fundamental is not visible because the spectra start at 150 kHz.
