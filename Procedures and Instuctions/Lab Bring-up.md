# Lab Bring Up 
This page has information about what to do in the event of power outages or power interruptions. 
## Ion Control Server
Aquamarine uses Ion Control system to control all the modulators and triggers to run an experiment. This control system is currently in a VM on [Dell PowerEdge R720](https://i.dell.com/sites/content/shared-content/data-sheets/en/Documents/Dell-PowerEdge-R720-Spec-Sheet.pdf) Rack-mounted server.  
To start it up after the power shutdown follow these steps:
1. Attach a monitor to the D-SUB port on the front panel (shown in the image below), you will also need a keyboard and a mouse. 
2. Now power ON the server by pressing the power button on the front panel (shown in the image below). The power button should light green after clicking it once. 
	- If the power button does not light green, we will need to drain the power in the following way:
		* Detach all the cables (power, ethernet, USBs etc.) from the server front panel and back panel. (Remember to take a picture of attachments before unplugging)
		* Now press and hold the power button of the server for about 30 seconds. 
		* Now you can plug all the cables back in the same configuration. 
		* Once the power cables are plugged in you can press the power button once and turn on the server. 
![[IMG_5751.jpeg]]
3. Now you should see the monitor screen light up and start the boot process. During this process it will display on the monitor that the iDRAC chip isn't recognized or configured, and it will as you to hit F1 on the keyboard to continue or F2 on the keyboard to open boot setup. Hit F1. At this time the fans of the server should spin much faster with louder noise. (You will be asked to Hit the F1 key on the keyboard twice, do it both times). 
4. Then Monitor will show the Debian boot up screen and it will ask you for the login and password, use the following:
	```
	login: vboxadmin
	password: Tr@ppyT0wn
	```
5. After booting the the server you have two options:
	- Start the Virtual box on the server itself using the attached monitor and keyboard or use XMing on the lab PC to start and display the virtual box. 
		- If you decide to do the later follow these steps:
			1.  Open a putty session on the lab PC (MobaXTerm is installed you can open that). And ssh into `senkolab-server` the login and pass are same as stated above. 
			2. Once logged in start up `virtual box` - a window like this should open up. ![[Screenshot 2026-04-09 at 4.03.33 PM.png]]
			3. The VM that hosts IonControl is called `senkolab-controller` 
				- if its not already `Running`, right-click on the `senkolab-controller` and go to `Start` and clock on `Normal Start`. 
				- You should see a new window open up which shows Windows 10 start up screen. Wait until its done booting up and you are asked to enter credential (after hitting the spacebar). 
				- Now right click on the `senkolab-control` in the `VirtualBox Manager` again and go to close, hit `Save State`. A window with a loading bar should appear. 
				- Ones the save state is complete, right-click on the `senkolab-controller`, go to start and hit `Headless Start`.
6. Ones the `senkolab-controller` status says `Running` (like it does in the window above) you can use the `Windows Remote Desktop` to access that VM on the lab PC.
	- Select the PC name `senkolab-server:23389` and use the following credential to login:
		`login: ions`
		`password: Tr@ppyT0wn`
This should start up the VM that hosts the IonControl. 

## Laser Stabilizer VM

Aquamarine lab uses a homebuilt laser stabilizer setup to stabilize the power of 493nm laser. This was setup by Pei-Jiang Low using two RaspberryPis. The control software for these stabilizers is on VM hosted on the same server as IonControl VM. Follow these steps to get that VM up and running:
1. Follow steps 1-5 from [[#Ion Control Server]], in step 5 start `senkolab-laser-stabillizers` VM.
2. Ones the VM has started you can connect to the VM from `Windows Remote Desktop`. Connect to PC name `senkolab-server:43389` with the following credentials to login:
	`login: senkolab`
	`password: Tr@ppyT0wn`
To procedure to start up the laser stabilizer program is described here [[493nm Laser Stabilizer Bring up]] 
## HighFinesse Laser Control Server
Aquamarine lab uses [HighFinesse WS-7](https://www.highfinesse.com/en/wavelengthmeter/index.html#productfinder-wavelegthmeter) wavelength meter to measure and lock the frequencies of 493nm, 650nm, 554nm 614nm and 389nm lasers. The Control program for this wavemeter is on [Dell PowerEdge R410](https://i.dell.com/sites/csdocuments/Shared-Content_data-Sheets_Documents/en/R410-SpecSheet.pdf). This server runs windows so startup for this should be straight forward. 

![[IMG_5756.jpeg]]

Turn on the server and login with the following credentials 
```
login: ions
password: Tr@ppyT0wn
```

The Startup for Wavelength meter WS-7 program is described in [[Highfinesse WS7 laser control]].
