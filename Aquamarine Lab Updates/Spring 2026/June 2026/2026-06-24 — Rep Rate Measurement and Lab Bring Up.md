### [[Raman Bring up]]
In the last few weeks we were able to turn on the Raman Laser by replacing the desiccant and getting rid of Humidity error. 
We borrowed a fast photodiode from from Quantum Ion (Alphalas UPD-35-UVIR-P). 
We were able to measure the rep rate on the oscilloscope to be around 76.66MHz after putting the signal through a BiasTee and a MPA-40-40.

![[Pasted image 20260624130420.jpg|559]]

We were then able to measure the frequency on spectrum analyzer and saw the drift over a 20min period, here is the timelapse video. 

![[IMG_5915.mp4|200]]

The central frequency of the rep-rate seems to be about $\sim$ 75.66255MHz. 

I was then able to feed the rep-rate into [[RFSoC]] and measure the repetition rate there using technique described in [[RFSoC]]. 
These are the results of measuring the 10MHz clock signal (as a control), 1st, 2nd and 3rd Harmonic of the Rep-rate signal. 

**10 MHz - Clock Frequency**
This signal is coming from the same clock we are using as a reference for RFSoC ADC and DAC. So we expect there to be no frequency drifts in signal. 

After measuring for 30 seconds with step size of $\sim$ 2ms we get:

```
Summary:
points:           12605
mean frequency:   9999999.999490043 Hz
mean f_err:       -0.000510 Hz
std f_err:        0.001131 Hz
min f_err:        -0.005000 Hz
max f_err:        +0.004000 Hz
pk-pk f_err:      0.009000 Hz
std fractional:   1.131e-10
std ppb:          0.113 ppb
mean |IQ|:        180954617.6
```

| ![[Pasted image 20260624134313.png\|413]] | ![[Pasted image 20260624134701.png\|257]] |
| ----------------------------------------- | ----------------------------------------- |

So we see that the frequency measurement has a resolution of 1mHz. The mixed signal does not oscillate because we are mixing the same frequency. 

So this gives us confidence that we can measure any frequency, give that we adjust the Local Oscillator frequency to be near the expected frequency of the input signal. 

**75 MHz - 1st Harmonic**
Next we put in the Rep-rate signal in the RFSoC and measure for 30 seconds with step size of $\sim$ 2ms.
```
Summary:
points:           12667
LO Frequency:     75662550.00000063
mean frequency:   75662489.308102384 Hz
mean f_err:       -60.691898 Hz
std f_err:        4.598685 Hz
min f_err:        -69.926000 Hz
max f_err:        -50.256000 Hz
pk-pk f_err:      19.670000 Hz
std fractional:   6.078e-08
std ppb:          60.779 ppb
mean |IQ|:        24893161.7
```

```
Residual statistics after polynomial subtraction:
mean residual:       -0.000000000 Hz
std residual:        0.180169237 Hz
sample std residual: 0.180176349 Hz
pk-pk residual:      1.377472750 Hz
```

| ![[Pasted image 20260624135940.png]] | ![[Pasted image 20260624140019.png\|283]] |
| ------------------------------------ | ----------------------------------------- |

So with the actual Rep-Rate signal we are able to measure the frequency with 0.2Hz of resolution for the first Harmonic frequency. 
This is with $\sim$ -11dbm of power going to the RFSoC. 
![[Pasted image 20260624144524.jpg|520]]

**151MHz - 2nd Harmonic**

Now we measure the second harmonic frequency:
```
Summary:
points:           12572
LO Frequency:     151325099.99999952
mean frequency:   151324927.914788604 Hz
mean f_err:       -172.085211 Hz
std f_err:        7.601589 Hz
min f_err:        -190.292000 Hz
max f_err:        -154.636000 Hz
pk-pk f_err:      35.656000 Hz
std fractional:   5.023e-08
std ppb:          50.233 ppb
mean |IQ|:        19674144.1
```

so we see about twice the standard deviation from first harmonic which is expected since any rep-rate drift will be twice at the second harmonic. 

```
Residual statistics after polynomial subtraction:
mean residual:       +0.000000000 Hz
std residual:        0.420644238 Hz
sample std residual: 0.420660968 Hz
pk-pk residual:      4.013637061 Hz
```

| ![[Pasted image 20260624145028.png]] | ![[Pasted image 20260624145057.png\|292]] |
| ------------------------------------ | ----------------------------------------- |

**227MHz - 3rd Harmonic**
And finally measuring the third harmonic:

```
Summary:
points:           12648
LO Frequency:     226987650.00000015
mean frequency:   226987414.721379489 Hz
mean f_err:       -235.278621 Hz
std f_err:        15.860313 Hz
min f_err:        -269.608000 Hz
max f_err:        -195.798000 Hz
pk-pk f_err:      73.810000 Hz
std fractional:   6.987e-08
std ppb:          69.873 ppb
mean |IQ|:        9039306.8
```

```
Residual statistics after polynomial subtraction:
mean residual:       +0.000000000 Hz
std residual:        0.580705226 Hz
sample std residual: 0.580728184 Hz
pk-pk residual:      4.913351434 Hz
```

| ![[Pasted image 20260624145414.png]] | ![[Pasted image 20260624145448.png\|292]] |
| ------------------------------------ | ----------------------------------------- |
So we see the increase in amplitude of fast jitter directly proportional to the number of harmonic (going from $\sim$ 0.2Hz to $\sim$ 0.4Hz to $\sim$ 0.6Hz). 



