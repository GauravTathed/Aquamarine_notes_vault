### [[Brightness Fluctuations]]
For last couple of days we have seen that brightness of Ba137 fluctuates by around 30-40%. We replaced the 8GHz switch on 493nm EOM RF Signal to see if this will solve the issue. It didn't. 

We have also see inconsistent response to fluorescence frequencies and EOM frequencies in terms of brightness and cooling efficiency. 

Todays response:

| Frequencies | Description      | Power   |
| ----------- | ---------------- | ------- |
| 607.43201   | 493 nm           | 56 uW   |
| 461.31134   | 650 nm           | 250 uW  |
| 8.1 GHz     | 493 EOM sideband | -15 dbm |
Ion Brightness 450 counts/ 100ms. Cooling: not good. 

Fluctuations and cooling:
![[Pasted image 20260811132155.png]]
Nico found two lenses on 493nm AOM boards to be loose. Fixed those in place and aligned 493 back into collimator, Ions seems more stable now. 

#dailycalibrations 
![[Pasted image 20260811153903.png]]

### Raman Ramsey Measurements:

**Clock transition: S20S10**
This is the Data with [[2026-07-15 — Rep-Rate Stabilization]] ON:

| Description                                 | Plots and Figures                                                                                      |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Raw Plots of phase scans at each wait time: | ![[Pasted image 20260812005033.png]]                                                                   |
| Fitting the Contrast:                       | ![[Pasted image 20260812010703.png]]                                                                   |
| Phase Drift over time:                      | ![[Pasted image 20260812010504.png]]                                                                   |
| Fitted Parameters:                          | `T2 Gaussian: 808.91 ± 81 us`<br>`T2 Lorentzian: 1272.8 ± 266 us`<br>`Effective T2*: 591.72 ± 17.9 us` |


**S21S11 - $\kappa \sim$ 0.7 MHz/G**

Found this transition @ 8031.85285 MHz + 0.085 MHz. With Pi time of $\sim 88 \mu$s. 

| Description                                 | Plots and Values                                                                                                  |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Raw Plots of phase scans at each wait time: | ![[Pasted image 20260812011309.png]]                                                                              |
| Fitting the Contrast:                       | ![[Pasted image 20260812011323.png]]                                                                              |
| Phase Drift over time:                      | ![[Pasted image 20260812011340.png]]                                                                              |
| Fitted Values:                              | `T2 Gaussian: 430.42 ± 41.8 us`<br>`T2 Lorentzian: 2.7215e+09 ± 8.46e-13 us`<br>`Effective T2*: 430.42 ± 41.8 us` |
