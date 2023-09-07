---
date: 2023-09-06
tags:
  - steamdeck
  - tutorial
  - moonlight
  - gyroDSU
  - cemu
---
# What is this
Since I only have an XBox One controller and that controller does not have gyro capability, it is very hard for me to play Zelda BOTW.
Some shrines in BOTW require to use gyro. There are other options like using a PS4 or PS5 controller or use your **Android** phone as a gyro device but I like when everything work on one device so it does not feel "hacky". I did not test how fluent SteamDeck run BOTW natively, but I am using Sunshine and Moonlight to stream CEMU from my much more powerful PC.

# How it works
Rendering is done on your PC. Basic control and gyro motion are passthru from your SteamDeck to your PC.
This works because of the following:
- [SteamDeckGyroDSU](https://github.com/kmicki/SteamDeckGyroDSU) will host a very lightweight server on your SteamDeck and that server is used to transmit gyro motion to your PC Cemu emulator.
- Your normal control, like DPAD, ABXY, and Joystick are passthru via Moonlight
If you have not setup Moonlight and start rendering game from your pc, you should definitely set it up [[Sunshine Moonlight and Moondeck Buddy]]
![[Pasted image 20230905201552.png]]

# Setup
You will need to get Moonlight working first before following this guide. Moonlight is a streaming application and you cant see anything from your PC without streaming, so set that up first [[Sunshine Moonlight and Moondeck Buddy]]

## SteamDeckGyroDSU (SteamDeck)
This portion require SteamDeck Desktop mode, a tiny bit of terminal interaction, and you should also already have `sudo` password set.
### Go to Desktop
1. Press on Steam button on left hand side of your SteamDeck (below trackpad), then go to Power
![[20230905205943_1.jpg]]
2. Select Switch to Desktop
![[20230905205946_1.jpg]]
### Open Konsole Terminal
You can click on bottom left corner start menu, then `System` - `Konsole`
![[Pasted image 20230905210217.png]]
### Set sudo password for user deck
Open up a Konsole terminal, and type in
```bash
passwd
```
Then it will prompt you to set a new password
- Since I already set a password for user `deck`, it will ask current password before setting a new password. 
	- If you see it asking for your current password then you already have a `sudo` password set.
![[Pasted image 20230905204904.png]]
**DO NOT FORGET THIS PASSWORD, YOU WILL GO THROUGH PAIN IF YOU FORGET THIS**

### Install GyroDSU
This is a plugin you can either get from [SteamDeckGyroDSU](https://github.com/kmicki/SteamDeckGyroDSU) (follow their official README.md guide) or if you have EmuDeck already installed, you can launch EmuDeck installer and install from there.
#### Github method
Just follow their [README.md](https://github.com/kmicki/SteamDeckGyroDSU)If you are not very technical, just use the EmuDeck method, it is a bit easier.
#### Emudeck method
1. Start EmuDeck from your desktop, if you dont have this icon, you can download from [EmuDeck website](https://www.emudeck.com/EmuDeck.desktop)
![[Screenshot_20230905_203643.png]]
2. Once you (re)install EmuDeck, start EmuDeck and in main dashboard, locate Gyroscope and click on **More Info**
![[Pasted image 20230905204626.png]]
3. You will need to set a `sudo` password for user `deck` if you have not done so already. [[#Set sudo password for user deck]]
4. Then you can enter the `sudo` password you just created into this prompt
![[Screenshot_20230905_203721.png]]
5. Then you will see a terminal shell will open, just let it run
![[Screenshot_20230905_203734.png]]
6. Once you see Installation done, then you can close this terminal. **DO A SYSTEM RESTART** even though this screenshot does not prompt me for restart, it did prompt me the first time I install this plugin.
7. Once you restart your SteamDeck, go back to Desktop mode, open a Konsole terminal and type in this command:
```bash
lsof -i:26760
```
- This command will show that GyroDSU is running and it is running on port 26760. We will connect to this port on our PC Cemu. This service will be running even when we go to Game Mode or restart SteamDeck
![[Screenshot_20230905_203931 1.png]]
### Uninstall GyroDSU
If you ever want/need to uninstall GyroDSU, you can use this command from their official [Github Repo](https://github.com/kmicki/SteamDeckGyroDSU)
```bash
$HOME/sdgyrodsu/uninstall.sh
```
## Enable Gyro (SteamDeck)
Now that our GyroDSU service is running properly, we can return to SteamDeck's game mode, launch Moonlight shortcut and configure Game Layout
1. Go to Moonlight shortcut, click on the game pad, this button is for configuring layout
![[Pasted image 20230905205659.png]]
2. Click on Edit Layout 
![[20230905205611_1.jpg]]
3. Go to Gyro tab and enable **Directional Pad** This needs to be Directional pad otherwise it will not work
![[20230905204041_1.jpg]]
## Get SteamDeck LAN IP address (SteamDeck)
- We will need to get SteamDeck local network IP address in order for Cemu to connect.
- You can go to `Setting` -> `Network` -> Select your WIFI
- No worry, this is local only, no one will hack you by obtaining this IP... just want to point this out
![[Pasted image 20230905215603.png]]

## CEMU (PC)
We will now configure Cemu emulator on PC
1. Open up `Cemu`, `Options` -> `Input settings`
![[Pasted image 20230905215858.png]]
2. Under `Controller 1` profile, you want to setup an `emulated controller` of `Wii U GamePad`. Only `Wii U GamePad` support gyro motion
3. Then in Controller (this is your actual physical controller) we will setup basic control first
**Note, YOU MUST CONFIGURE THE CONTROLLERS IN THIS ORDER, REVERSE ORDER WILL MAKE GYRO NOT WORK**
XINPUT FIRST, THEN GYRO/DSUController
![[Pasted image 20230905223522.png]]
### XINPUT
We will need to get basic key mapping into Cemu, and surprisingly, Cemu allow you to use multiple controller input on the same controller profile (one for basic control and one for gyro)
**YOU WILL NEED TO LAUNCH MOONLIGHT INORDER FOR CEMU TO DETECT STEAMDECK BASIC CONTROL**
1. Click on the plus button and add a `XINPUT` API
![[Pasted image 20230906184403.png]]
2. It will say `Controller 1`, select and Add
![[Pasted image 20230905221356.png]]
3. There you go, all your basic mapping should be here, change A/B X/Y accordingly. If you move your joystick, the red dot should be moving accordingly
![[Pasted image 20230905221520.png]]
**Note, you can only change your basic control mapping in XINPUT, If you trying to change basic control in Gyro/DSU controller it will have basic key mapping grayed out**
![[Pasted image 20230905221757.png]]

### DSUController
1. Click on the plus icon next to the drop down, and select API `DSUController`
![[Pasted image 20230906184606.png]]
2. Change the IP to your SteamDeck IP, for me it is `192.168.1.56`
3. After a brief moment, `Controller 1`(SteamDeck's `GyroDSU`) will show up
	 ![[Pasted image 20230905220330.png]]
 4. Select `Controller 1` and click Add
 5. Then while you still have `Controller 1 [DSUController]`, click `Setting` -> `Use Motion`, then Ok
![[Pasted image 20230905220516.png]]
At this point Gyro is working and we should be able to move around from tilting our SteamDeck, you also got basic control from XINPUT
4. Give this controller a name and save
![[Pasted image 20230905221841.png]]