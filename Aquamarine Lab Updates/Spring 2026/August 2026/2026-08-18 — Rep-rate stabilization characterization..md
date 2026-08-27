The following data shows the beat note frequencies on field fox tracked over several minutes. 

**106th tone - 8037.7503 MHz**
For this measurement FieldFox wasn't attached to the external Clock

|                | With Rep-rate Stabilization          |
| -------------- | ------------------------------------ |
| Spectogram     | ![[Pasted image 20260818221953.png]] |
| Peak Frequency | ![[Pasted image 20260818222047.png]] |
| PDF            | ![[Pasted image 20260818224050.png]] |

With Clock attached to the FieldFox:

|                | Stabilization ON                     | Stabilization OFF                    |
| -------------- | ------------------------------------ | ------------------------------------ |
| Spectogram     | ![[Pasted image 20260818222930.png]] | ![[Pasted image 20260818230036.png]] |
| Peak Frequency | ![[Pasted image 20260818222948.png]] | ![[Pasted image 20260818230114.png]] |
| PDF            | ![[Pasted image 20260818223833.png]] | ![[Pasted image 20260818230348.png]] |

with rep-rate stabilization ON the beat frequency is exactly where we expect it to be:
$$75.66255\times106 + 17.52 = 8037.7503\,\text{MHz}$$ The FWHM of fitted Voigt profile is 1.53Hz when we rep-rate lock the frequency. 

So ig we can compare these numbers with QITI. 

I am kind of curious to see how the FWHM of PDF scales with mode that we are stabilizing. 

so we will go down in steps of 25 modes and add an offset of $12.345678\,\text{MHz}$.

**81st Mode:**

|                | Stabilization OFF                    | Stabilization ON                     |     |
| -------------- | ------------------------------------ | ------------------------------------ | --- |
| Spectogram     | ![[Pasted image 20260818234925.png]] | ![[Pasted image 20260819003346.png]] |     |
| Peak Frequency | ![[Pasted image 20260818234940.png]] | ![[Pasted image 20260819003412.png]] |     |
| PDF            | ![[Pasted image 20260818234959.png]] | ![[Pasted image 20260819003319.png]] |     |
