---
date: 2023-08-21
tags:
  - steamdeck
  - tutorial
  - moonlight
---
# What is this
Since Nvidia Shield GameStream is going away soon if not already https://www.androidpolice.com/nvidia-shield-tv-sunset-gamestream/. Awesome open source developers from (LizardByte, FrogTheFrog and Moonlight-stream) created a new way of utilizing Moonlight and Sunshine stack to stream game to your handheld devices within local network. To my surprise, it is easier to setup Sunshine and Moonlight than going through Nvidia shield route.

# How it works
Your pc is hopefully, more powerful than the steamdeck. We will use that PC to do all the hard work (rendering) and stream the image to our SteamDeck. Our SteamDeck will display what PC rendered, and sending input signal back to PC. To my surprise, this process have very low latency and is very reliable (at least in local LAN environment).
- You still need Moonlight and Sunshine (Nvidia GameStream replacement) as the bare minimum. They are the core component transmitting the stream
- MoonDeck and Moondeck Buddy are helper shortcuts to launch game directly from SteamDeck library page, no need to manually add game to Moonlight dashboard

![[Pasted image 20230821164505.png]]

# Setup
## Sunshine (Nvidia Shield GameStream replacement) (PC)
This will be a service running on your powerful computer. A service (in the background) and provide streaming functionality when a Moonlight device trying to connect. It have a localhost website where you can configure many settings.
In many way this is better than Nvidia Shield GameStream due to the following:
- It support all 3 major vendor of GPU, Nvidia, AMD, Intel
- Hostable on Windows, Linux, and MacOSX

### Install
Setup for sunshine is the most difficult part compare to the rest of the tutorial but it is totally doable.
1. Download latest Sunshine binary from Github: https://github.com/LizardByte/Sunshine/releases/latest, I am on Windows and I will be downloading the Windows installer version. Portable avoid installing to your system registry. It is a preference.
2. Launch installer and use default setting and finish installation process.

### Setup
Now you should see a Sunshine icon on your bottom right hand tray, if you do not see the icon try launching from start menu.

![[Pasted image 20230821142125.png]]

![[Pasted image 20230821142222.png]]
1. Single left click on Sunshine icon and click on Open Sunshine 
![[Pasted image 20230821142301.png]]
2. You will see an insecure warning, it is ok since this is only hosting on your local network, we can safely dismiss this warning.
![[Pasted image 20230821142549.png]]
3. You will be prompt for an admin username and password, this is the credential you will use to make change to Sunshine. 
4. After setup is complete, page will refresh and ask for your credential you just set.<br/>
![[Pasted image 20230821142722.png]]


5. This is the home page for Sunshine, you wont be needing to see this dashboard after everything is setup.<br/>
![[Pasted image 20230821142835.png]]

Minimize this page we will comeback to Sunshine shortly

## MoonDeck and MoonDeck Buddy
MoonDeck Buddy(PC) is a companion app to MoonDeck(SteamDeck) . MoonDeck is a shortcut launcher (on Steamdeck) for launching a Moonlight session without going through Moonlight dashboard and click on game then start. It is an icon on Steamdeck game page and you click on icon to start game
### Decky Loader and MoonDeck (SteamDeck)
1. You can install Decky Loader by following the installation guide on their Github https://github.com/SteamDeckHomebrew/decky-loader
2. If install successfully, when you press ... on your SteamDeck at the bottom there should be this Decky Loader icon<br/>
![[20230821144058_1.jpg]]
3. From there you can go to the Store/Market and install MoonDeck plugin<br/>
![[20230821144107_1.jpg]]
4. After installing MoonDeck plugin, this icon should show up on all your Steam games.<br/>
![[Pasted image 20230821143942.png]]
5. This means your MoonDeck plugin is installed successfully, you will need to configure the receiver application on the pc side

### MoonDeck Buddy (PC)
1. Install MoonDeck Buddy on https://github.com/FrogTheFrog/moondeck-buddy/releases with default parameters
2. Then run the MoonDeck Buddy from start menu: <br/>
![[Pasted image 20230821144918.png]]
3. Firewall prompt, just click Allow Access<br/>
![[Pasted image 20230821144954.png]]

4. You might want to start this application on startup, right click then select start on system startup
![[Pasted image 20230821145044.png]]

## Moonlight(SteamDeck)
### Install
You will also need to install Moonlight in Steamdeck Desktop mode in order to pair with Sunshine.
MoonDeck and MoonDeck Buddy are only making Moonlight startup process easier, at its core, Moonlight is the one doing streaming.
1. Switch to desktop mode<br/>
 ![[20230821151714_1.jpg]]
 2. Go to Discover store and install Moonlight <br/>
 ![[Screenshot_20230821_162419.png]]
 ### Configure
 #### Basic
 1. Launch Moonlight and click on the Desktop name where you have Sunshine running<br/>
 ![[Screenshot_20230821_162513.png]]
 2. It will ask you to enter PIN on Sunshine interface<br/>
 ![[Screenshot_20230821_162526.png]]
 3. Go to your PC and type the 4-digit PIN onto Sunshine PIN tab<br/>
 ![[Pasted image 20230821162607.png]]
 4. If you click on your computer name now and see some application showing up, you are good to go, dont worry if you dont have MoonDeckStream right now, we will set that up very soon<br/>
 ![[Screenshot_20230821_162642.png]]
#### Quit Moonlight
It is worth noting that quitting a game by pressing Steam key - Exit by default will only exit Moonlight application, stream is on pause but MoonDeckStream process still alive. To terminate the stream completely and __potentially__ quit the game on host machine do the following. 
1. In Moonlight, go to settings
2. Check Quit app on host PC after ending stream.

This setting will kill MoonDeckStream process. I have mix result if the game will quit when this option is checked. Crysis3 does not quit even with this checkbox. But Ryujinx does quit.<br/>
![[Pasted image 20230821224310.png]]

 ### Add as Non Steam Game
 1. Add Moonlight as Non Steam game is also helpful if you need to start Moonlight for any reason (like launching an emulator located on your PC) without going to SteamDeck desktop mode<br/>
 ![[Screenshot_20230821_162720.png]]
 2. You should see Moonlight as an item in Library <br/>
 ![[Screenshot_20230821_162810.png]]
3. Return to SteamOS

## Sunshine (cont.) (PC)
### Adding application
1. Per documentation, we need to add MoonDeck Buddy on Sunshine to get MoonDeck working. It is also a good idea to add other major launcher you will use into the application tab, I added Yuzu and Ryujinx.
2. Click on Applications - Add New and enter the following information:![[Pasted image 20230821150225.png]]
#### MoonDeckStream:
- Application Name: MoonDeckStream
- Command: PATH TO YOUR MOONDECK BUDDY
	 - For me, it is: C:\\Users\\USERNAME\\AppData\\Local\\Programs\\MoonDeckBuddy\\bin\\MoonDeckStream.exe
3. Save
4. Your application should have 3 items now, 2 are default and 1 you just add<br/>
![[Pasted image 20230821150536.png]]
#### Yuzu/Ryujinx:
It will be exactly the same, just replace the Application name with Yuzu/Ryujinx, and set the path to the .exe file

## MoonDeck Buddy (SteamDeck)
### Pairing
Lastly we will need to pair MoonDeck with MoonDeck Buddy. This is to make the shortcut start game on desktop pc
1. Go to MoonDeck plugin on Decky Loader<br/>
![[20230821165906_1.jpg]]
2. Go to Settings<br/>
![[20230821165911_1.jpg]]
3. Click on Scan and choose the same PC you setup for Moonlight<br/>
![[20230821165919_1.jpg]]
![[20230821165937_1.jpg]]
4. If it is your first time setting up MoonDeck Buddy, click on Pair at the very bottom. Another 4-digit PIN will show and you will need to enter into MoonDeck Buddy prompt, NOTE INTO MOONDECK BUDDY PROMPT, NOT SUNSHINE (MoonDeck Buddy prompt should show up automatically on your PC once you click on Pair)<br/>
 ![[20230821165940_1.jpg]]

### Add Moonlight application to SteamOS
If you want all the application show up on SteamOS as Non Steam game, you can do so by selecting Sync all Sunshine's apps via Buddy<br/>
![[20230821221217_1.jpg]]<br/>
This sync will be based on items you defined in Application tab, for me I have 3 items and they are the following.

Note: Since MoonDeckStream is using via MoonDeck shortcut, Sync will omit this shortcut and only give us 2 icons (Ryujinx and Yuzu)<br/>
![[Pasted image 20230821221104.png]]<br/>
Note: these two icons (Ryujin and Yuzu) are for streaming from Sunshine PC, it should not be confused with Yuzu and Ryujinx on the SteamDeck. 

I have independent emulated games in this collection due to Steam Rom Manager https://github.com/SteamGridDB/steam-rom-manager/releases. 

Note2: Steam Rom Manager will only parse ROM on SteamDeck.<br/>
![[20230821221249_1.jpg]]
### SteamGridDB
I will assign Moonlight Ryujinx and Yuzu new icons so they are distinguishable. I am doing this with SteamGridDB plugin. It is in the Decky Loader market place.<br/>
![[20230821232159_1.jpg]]

This plugin can assign icons/hero image/capsule artworks to non steam games. The end result is the following<br/>
![[20230821222139_1.jpg]]

# Running a game
You have complete all the setup and its time to launch a game. 
1. Click on the MoonDeck icon on a game<br/>
![[Pasted image 20230821143942.png]]
2. If should show it is starting MoonDeckStream (which we just add as a Sunshine application)<br/>
![[most_recent.jpg]]
3. Now you have butter smooth 60FPS on most your games (of course, this depends on how powerful your PC is, but on a 800P screen, it most likely have better FPS than your computer's 1080P 1440P or 4K)
![[20230821173231_1.jpg]]
![[20230821173318_1.jpg]]

# Switch HDR
If your monitor support HDR and you have it on by default, most likely you will get over exposure when streaming to SteamDeck
![[20230821225432_1.jpg]]<br/>
To solve this issue, samglit got a solution that I happy to share https://old.reddit.com/r/SteamDeck/comments/11j22ob/remote_streaming_for_moonlight_sunshine_moondeck/

He suggest a program called AutoActions, which will monitor a process creation/termination and perform action accordingly.
Auto action Github: https://github.com/Codectory/AutoActions/releases/tag/1.9.19

## Profile
In profile I added 2 profiles
1. When MoonDeckStream process is detected, turn off HDR, set Refresh Rate to 60 and set Resolution to SteamDeck native 800P. <br/>
![[Pasted image 20230821230029.png]]

2. Revert to my own setting once MoonDeckStream process is terminated<br/>
![[Pasted image 20230821230116.png]]

## Hook
Now I only have to hook this profile to process I wish to monitor. Now when I use Moonlight to start any of the 3 process, the hook will trigger accordingly<br/>
![[Pasted image 20230821230241.png]]


# Disclaimer
The content provided in this tutorial is intended for informational purposes only. It is not intended to be a definitive guide, professional advice, or a guarantee of any outcomes. The methods, techniques, and instructions outlined in this tutorial may involve technical procedures that could potentially impact your machine or system.

It is important to note that every computer environment is unique, and the steps outlined in this tutorial may not be suitable for all situations or configurations. The author of this tutorial does not assume any responsibility for any potential consequences that may arise from implementing the instructions or recommendations presented herein.

Before attempting any actions described in this tutorial, it is strongly recommended that you fully understand the potential implications of those actions on your system. If you are uncertain about any steps or techniques, it is advisable to seek professional assistance or conduct thorough research before proceeding.

By using this tutorial as a reference, you acknowledge that the author is not responsible for any damages, losses, or disruptions that may occur as a result of following the information provided. Your use of this tutorial is entirely at your own risk, and you agree to take full responsibility for any outcomes that may arise from your actions.

Remember that technology environments are complex and constantly evolving, and changes made to your system based on the information in this tutorial could have unintended consequences. Always exercise caution, perform backups, and consider seeking expert advice when in doubt.