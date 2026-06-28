The update rate we were working with was $\sim$ 1kHz. And we were seeing the following uncertainties in our measurement. 

**1 kHz**

| Signal                | Frequency / LO | Slow std estimate | Fast std / residual | Fast std fractional |
| --------------------- | -------------: | ----------------: | ------------------: | ------------------: |
| 10 MHz clock control  |  10.000000 MHz |                 — |                   — |                   — |
| Rep-rate 1st Harmonic |  75.662550 MHz |       4.595154 Hz |         0.180169 Hz |            2.38 ppb |
| 2nd Harmonic          | 151.325100 MHz |       7.589942 Hz |         0.420644 Hz |            2.78 ppb |
| 3rd Harmonic          | 226.987650 MHz |      15.849679 Hz |         0.580705 Hz |            2.56 ppb |

| Signal      | Frequency / LO | Slow std estimate | Fast std / residual | Fast std fractional |
| ----------- | -------------: | ----------------: | ------------------: | ------------------: |
| AWG 10MHz   |         10 MHz |       0.042204 Hz |         0.004765 Hz |           0.021 ppb |
| AWG 75 MHz  |  75.662550 MHz |       0.374626 Hz |         0.035698 Hz |           0.472 ppb |
| AWG 151 MHz | 151.325100 MHz |       0.986976 Hz |         0.072653 Hz |           0.480 ppb |
| AWG 227 MHz | 226.987650 MHz |       1.406641 Hz |         0.105962 Hz |           0.467 ppb |

Now we can compare these error rates to $\sim$ 10kHz update rate. 

**10 kHz**

Now we read out frequency every 100$\mu$s. We expect the measurement uncertainty to be 10 times as bad. Just based on this this formula:
$$
f_{\text{err},k} = \frac{\Delta \phi_k}{2\pi T_{\mathrm{win}}}.  
$$
So we will start with a baseline measurement of 10MHz clock:
**10 MHz — Clock Frequency**

Feeding the ADC 10 MHz clock signal that's used as ref clock. 

```
Summary:
points read:       94513
LO Frequency:      10000000.000000 Hz
window time:       0.000100000 s
expected update:   10000.0 Hz
captured span:     9.996800 s
mean read rate:    9454.3 samples/s
missed updates:    5456
mean frequency:    9999999.999498736 Hz
mean f_err:        -0.000501 Hz
std f_err:         0.023926 Hz
min f_err:         -0.102000 Hz
max f_err:         +0.107000 Hz
pk-pk f_err:       0.209000 Hz
std fractional:    2.393e-09
std ppb:           2.393 ppb
mean |IQ|:         18079151.3
```

```
Residual statistics after polynomial subtraction:
mean residual:       +0.000000000 Hz
std residual:        0.023925173 Hz
sample std residual: 0.023925299 Hz
pk-pk residual:      0.208872485 Hz
```

| ![[Pasted image 20260627215354.png]] | ![[Pasted image 20260627215411.png\|300]] |
| ------------------------------------ | ----------------------------------------- |
**75 MHz - 1st Harmonic**
Now feeding in rep-rate signal from photo-diode:
```
Summary:
points read:       46116
LO Frequency:      75662550.000000 Hz
window time:       0.000100000 s
expected update:   10000.0 Hz
captured span:     9.995400 s
mean read rate:    4613.7 samples/s
missed updates:    53839
mean frequency:    75662498.027565986 Hz
mean f_err:        -51.972434 Hz
std f_err:         3.171992 Hz
min f_err:         -59.408000 Hz
max f_err:         -43.473000 Hz
pk-pk f_err:       15.935000 Hz
std fractional:    4.192e-08
std ppb:           41.923 ppb
mean |IQ|:         2297842.4
```

```
Residual statistics after polynomial subtraction:
mean residual:       +0.000000000 Hz
std residual:        0.316065966 Hz
sample std residual: 0.316069393 Hz
pk-pk residual:      2.590048651 Hz
```

| ![[Pasted image 20260627215938.png]] | ![[Pasted image 20260627220030.png\|291]] |
| ------------------------------------ | ----------------------------------------- |
**151MHz - 2nd Harmonics**

```
Summary:
points read:       44372
LO Frequency:      151325100.000000 Hz
window time:       0.000100000 s
expected update:   10000.0 Hz
captured span:     9.996900 s
mean read rate:    4438.6 samples/s
missed updates:    55598
mean frequency:    151324971.460978121 Hz
mean f_err:        -128.539022 Hz
std f_err:         5.826011 Hz
min f_err:         -143.828000 Hz
max f_err:         -113.941000 Hz
pk-pk f_err:       29.887000 Hz
std fractional:    3.850e-08
std ppb:           38.500 ppb
mean |IQ|:         1977183.2
```

```
Residual statistics after polynomial subtraction:
mean residual:       -0.000000000 Hz
std residual:        0.630278965 Hz
sample std residual: 0.630286067 Hz
pk-pk residual:      5.250147457 Hz
```

| ![[Pasted image 20260627224706.png]] | ![[Pasted image 20260627224725.png\|292]] |
| ------------------------------------ | ----------------------------------------- |

**227MHz - 3rd Harmonic**

```
Summary:
points read:       44369
LO Frequency:      226987650.000000 Hz
window time:       0.000100000 s
expected update:   10000.0 Hz
captured span:     9.997100 s
mean read rate:    4438.2 samples/s
missed updates:    55603
mean frequency:    226987440.496434420 Hz
mean f_err:        -209.503566 Hz
std f_err:         6.861077 Hz
min f_err:         -229.737000 Hz
max f_err:         -196.820000 Hz
pk-pk f_err:       32.917000 Hz
std fractional:    3.023e-08
std ppb:           30.227 ppb
mean |IQ|:         1124554.3
```

```
Residual statistics after polynomial subtraction:
mean residual:       -0.000000000 Hz
std residual:        0.979578223 Hz
sample std residual: 0.979589262 Hz
pk-pk residual:      8.141236986 Hz
```

| ![[Pasted image 20260627225517.png]] | ![[Pasted image 20260627225538.png\|295]] |
| ------------------------------------ | ----------------------------------------- |
