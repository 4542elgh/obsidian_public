---
title: 3D Printer Slicer - Orca Slicer
tags:
  - 3d_printer
  - tutorial
  - 3d_printer_tuning
  - 3d_printer_slicer
---
# Purge Line/Priming line
That is a purge line. It is there to make sure filament is in the nozzle, and get out any old filament left from a previous print. I find it very convenient to remove any old oozing filament. It’s not a setting you can change in your slicer, it is in the starting gcode. You can just take out the lines of code that tell it to print the line if you want to get rid of it.

![[Pasted image 20231210211305.png]]
# Bed Adhesion
## Skirt
![[Pasted image 20240327200309.png]]
![[Pasted image 20231210211815.png]]
- Skirts serve a useful purpose because they help prime your extruder and establish a smooth flow of filament. It can often catch some random bobs on the nozzle and make a cleaner print. The skirt also gives you a sense of how your adhesion and first layer look before starting the print.
- The skirt for checking bed tram / z-height.
	- I set mine to 2 passes instead of 3, every now and then my z limit switch likes to be real wonky and I'd rather see it on the skirt and quickly adjust z-offset.
### When to use what foundation
![[Pasted image 20231210212028.png]]
- Raft - For bad beds, you should fix/upgrade the printer and not use this crutch.
- Skirt - Always. It helps get the nozzle ready and is sanity check. If there is any adhesion problem, this will show
- Brim - For improved adhesion on prints that have a small base but large height. Sometimes for other tricky prints and ABS.
	- I use a brim to stop warping and peeling too, especially on heated beds
# Speed
https://www.reddit.com/r/ender3/comments/16yrae1/ender_3_v3_se_speed_up_pla_printing/
# Tree support
## Enable tree support
![[Pasted image 20240327200215.png]]
## Support Threshold angle
Print an overhang test, if you see 60 degree is ok then you will want to enter 90 - 60.
This means degree > 60 will be having supports
![[Pasted image 20240316171101.png]]
Note overhang and bridging is not the same. Overhang does not have another endpoint to reach while bridging have two points the filament can rest on
## Bridging
3d printers _can_ print mid air as long as you have an anchor point. The printer pulls the material taught, keeping tension on it to keep a straight line, while the cooling fan cools the material behind the nozzle. This creates a _bridge_ that doesn't need supports!
![[Pasted image 20240328080750.png]]
# Color Change
For example this is the layer my printer finish printing first color
![[Pasted image 20240327195827.png]]
You will want to one up that layer then add Gcode M600 to do color swap. A rule of thumb is when you see the outline of your second color, that is the layer you want to add M600 for color swap
![[Pasted image 20240327195933.png]]
# Extrusion multiplier
This is another name for extrusion flow in Cura
YOU MUST DO E-STEP CALIBRATION BEFORE CALIBRATING EXTRUSION MULTIPLIER AND Z-OFFSET
If you cannot get reliable top layer with 0.9 multiplier, please redo your [E-Step](https://letsprint3d.net/how-to-calibrate-the-extruder-steps-ender-3-5-cr-10/)
![[Pasted image 20240327201319.png]]
## Example
![[Pasted image 20231226162223.png]]
### Bad
![[Pasted image 20231226161558.png]]
### Better(still a bit over extruding, you can still see the string lines)
![[Pasted image 20231226161527.png]]
# Z-hop (retract before moving nozzle)
- With Z-hop you don't get scars on top solid layers.
- Z hop also helps where you have tall narrow (tower like) features on your model. helps to prevent the extruder displacing the existing tower layers when the extruder attempts to put down the next layer.

![[Pasted image 20240327201231.png]]
# Object strength
## Vertical Perimeters (wall layers)
More wall count will increase model strength and integrity more than infill
![[Pasted image 20240327200910.png]]
## Infill
Use gyroid to avoid nozzle running into infill on same layer

![[Pasted image 20240327201037.png]]
