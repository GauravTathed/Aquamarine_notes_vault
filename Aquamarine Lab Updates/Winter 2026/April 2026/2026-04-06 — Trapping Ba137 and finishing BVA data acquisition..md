### [[Trapping Ba137]]
Trapping over the weekend was great we averaged under 20 attempts, ion lifetime seems to be OK too. 
#dailycalibrations 
![[Pasted image 20260406112805.png]]

Nico noticed that the Pi-Times are changing quite a bit and affecting the Randomized Benchmarking Data. This data is particularly sensitive to pi time errors since some unitaries will be implemented in manner where pulse time error add constructively and some destructively. 
### [[BVA]] Data

So we were taking BVA data for different QuDit dimensions with short U1 after state initialization instead of full QFT. 

This is the complete data set upto D=16. 

![[Pasted image 20260406114657.png]]

![[Pasted image 20260406114710.png]]

We can see a clear mismatch between the simulation and measured data for the Line Signal OFF (AC/LS compensation ON) case. The following could be the reason for this:

