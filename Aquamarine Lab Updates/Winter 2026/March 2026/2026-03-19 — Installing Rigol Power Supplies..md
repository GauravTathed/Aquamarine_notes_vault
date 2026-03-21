Today we are installing the new [Rigol DB832](https://www.rigolna.com/products/dc-power-loads/dp800/) power supplies for the Trap Rods and Needles. 

Quick Aside - This morning Nico found the Trap RF voltage set to 0V, so and when he turned it ON the reflected power on spectrum analyzer was higher than usual. 
- Looks like someone turned off the trapping script at the exact moment the ion control Toggles Trap ON and OFF by setting the voltage to 0 and back to 4. 
- The resonator needs some time to warm up and setting into the resonant frequency. That's why the reflected power was high. 

We noticed with Arduino that trapping lifetimes were noticeably low ($\sim$ 4-6 Hours), this is after doing the Micromotion compensation. Most likely explanation is that Arduino has nosier output voltage? 

We will see if the problem continues with the new power supplies (if it does then its not the power supplies and something else is broken). 

We were having a lot of issues:
- Crystalizing the Ion
- Ion changing positions
- Ion not responding to Rods

Decided to give up for the day and work on presentation for tomorrow. 