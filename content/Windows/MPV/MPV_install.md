---
tags: [windows, mpv]
title: MPV Setup
---

# Install MPV
Install MPV from [SourceForge repository | Windows](https://sourceforge.net/projects/mpv-player-windows/files/64bit/)
Set it as default for video player

![[Pasted image 20231022093043.png]]

![[Pasted image 20231022093122.png]]
# Bypass HDR over exposure in Windows
Issue: [Over-exposure in screenshots and recordings with HDR in Windows 11](https://answers.microsoft.com/en-us/windows/forum/all/over-exposure-in-screenshots-and-recordings-with/4b946bf2-c3a8-435f-9fff-e7fceb4b9949)
To overcome this you can use one of the two workarounds: 
## Disable HDR in Windows Settings
Disable HDR while watching a video

![[Pasted image 20231022093915.png]]
## Add settings to MPV config
Use Vulkan rendering engine in MPV (Vulkan does not support HDR)
We will be using a config file for MPV to switch rendering engine to Vulkan
1. Go to `%appdata%\mpv`

    ![[Pasted image 20231022094147.png]]

2. Create a `mpv.conf` with the following content:
This config file will use Vulkan API and MPV will automatically use English subtitle if it found one, disable/enable feature as you see fit
```conf
# Start in fullscreen mode by default.
# fs=yes

# Use a large seekable RAM cache even for local input.
# cache=yes

# Use extra large RAM cache (needs cache=yes to make it useful).
# demuxer-max-bytes=500M
# demuxer-max-back-bytes=100M

# Disable the behavior that the player will pause if the cache goes below a
# certain fill size.
# cache-pause=no

# Turn on English subtitle automatically
slang=eng,en

# Pervent HRD from over-saturation, (Vulkan dont have HDR)
vo=gpu
gpu-api=vulkan
```

# More features
Preview: 
- This bar will always be shown until you remove it in setting. I find this useful when watching series, for long movie I disable this feature.

    ![[Pasted image 20231022094700.png]]
- Peek is just preview into the past/future.

    ![[Pasted image 20231022095212.png]]
## Common setup
1. Find your MPV installation location. If your MPV is portable version, then go to your MPV folder. If you used an installer, you can search for MPV and open file location.

    ![[Pasted image 20231022094953.png]]
2. If `scripts` folder does not exist, create one
### Red progress bar
Paste `progressbar.lua` file into your `scripts` folder 
![[progressbar.lua]] 
### Peek
Paste `osc.lua` and `thumbfast.lua` into your scripts folder 
![[thumbfast.lua]]![[osc.lua]]