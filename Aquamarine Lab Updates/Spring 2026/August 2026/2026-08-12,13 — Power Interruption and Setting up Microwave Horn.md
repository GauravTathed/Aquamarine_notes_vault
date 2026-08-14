### Power Interruptions
Yesterday there was yet another generator test in the morning. We found everything on the UPS turned off again. Same as last time this happened [[2026-04-08, 09 — Power Interruptions, Trapping Ba137 and unstable Ion brightness]]. 

This time we have logs of UPS Charge and load during that time. This is what it looks like:

![[Pasted image 20260813132051.png]]
From the logs we see that the test started at 6:50 am and we don't have any logs after that. Meaning the UPS didn't power anything even for a short while. I tested this again at 9:53 am when the UPS still showed 100% charge but. I unplugged the UPS and everything on the UPS turned off, So I plugged it back and this time the charge showed 0%. 

So the UPS is definitely broken. We need top buy new one.  

This is what I settled on 
[Cyber Power OL1000RTXL2U](https://www.cyberpowersystems.com/product/ups/smart-app-online/ol1000rtxl2u/?_gl=1*17y5mv5*_up*MQ..*_ga*MzExNTQ0NDI0LjE3ODY2NzU5MDg.*_ga_43RK722MST*czE3ODY2NzU5MDckbzEkZzAkdDE3ODY2NzYwNjAkajE4JGwwJGgw) with an optional purchase of [compatible EBM](https://www.cyberpowersystems.com/product/ups/extended-battery-modules/bp36v60art2u/?_gl=1*15hfmve*_up*MQ..*_ga*MTQ5NzQyNTEyLjE3ODY2NzYyNzY.*_ga_43RK722MST*czE3ODY2NzYyNzYkbzEkZzAkdDE3ODY2NzYyODUkajUxJGwwJGgw). 

The UPS has a 3 internal battery each of 12V/9 Ah giving a total of 324Wh battery. With total load capacity of 900W. Our Current UPS displays a total load of 450W so the UPS alone can sustain our total load for about 20mins. 

If we purchase the EBM which has an additional 6 batteries each of 12V/9Ah giving a total of 648Wh we can sustain our load for about 1 hour 30 minutes.  And with each additional EBM we get an increase of about 1hr 20mins in runtime @ 450W.  
![[Pasted image 20260813231126.png]]

| Product            | Price (USD) |
| ------------------ | ----------- |
| UPS (OL1000RTXL2U) | 1235        |
| EBM (BP36V60ART2U) | 1055        |

### Ion Fluorescence issues
Just like last time when the generator testing happened and we lost the pressure in the chamber, the ion brightness is fluctuating likely from stary particles coming close or hitting the ion. 
![[Pasted image 20260813132734.png]]
Last time this lasted for one and a half days and then it fixed itself likely because the pressure went back down. 
We will wait and see if that happens this time as well.

The issues stopped at 2:30pm so basically on time. 
### Setting Up the Microwave horn
Before the power interruptions we were characterizing the coherence on our clock transition [S, 2, 0, S, 1, 0] using Raman and we found $T_2^*$ time to be around 600 $\mu$s. To confirm this is the issue with the Raman setup and not ion issue (like heating) we will use Microwave to drive the M1 transition between  [S, 2, 0, S, 1, 0]. 

This is the RF Path that the Microwave signal Follows:
![[rf_chain_diagram.png]]
And this is the placement of the horn close to the trap. ![[Image (20).jpg|637]]
### Trapping Ba137
The Fluorescence issues stopped. 

Trapped Ba137 at a new spot because we lost the position of older stop. We need to dial in the parameters because we are trapped dark ions very often. 

While working on Microwave one of the lead bricks dropped on 1762 optics or close to it. So the alignment was knocked out. 

So I aligned the 1762 just based on ion flopping with 1762 locked and EOM playing the correct frequency. The I fine tuned the alignment with an optically pumped transition. 

And finally did our #dailycalibrations 

![[Pasted image 20260814005837.png]]
#### Pressure issues

I think our pressure is reducing the ion lifetimes. And also introducing dark ions an hour or two after trapping. 

There was a dark Ion in the trap. Started re-trapping. 