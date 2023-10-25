---
tags: [python, kmk, keyboard]
title: KMK Setup
---


# Prefix
- Xiao RP2040 is a microcontroller roughly size of a MX switch with massive storage space

    ![[Pasted image 20231013215114.png]]
	- python code can be place inside storage without compiling (unlike QMK and ZMK) makes updating keymap easier and more accessible (no toolchain required). 
	- Easy to update keymap in `main.py`, configuration written in Python, easy to understand and modify
- Using [CircuitPython](https://circuitpython.org/) as base firmware. 
- [KMK](http://kmkfw.io/) is a library on top of CircuitPython. This library makes programming keyboard keycode and other features easy.

# You will need the following software
- [WinCompose](https://github.com/samhocevar/wincompose/releases/tag/v0.9.11) for Unicode support on Windows
- [MU Editor](https://codewith.mu/) or [VSCode](https://code.visualstudio.com/) for editing code (notepad++ or even notepad would work)
- [Putty](https://putty.org/) if you need to connect via COM port and see debug message

# Initial setup
The following setup only need to do once. If you got a working board already, you dont need to do this part. Skip to [[#Editor]] to setup editor or skip to [[#User Configuration]] to start configuring your keymap.
## Flashing CircuitPython base firmware .uf2

<div class="iframe-wrapper">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/S3ereO__N7Q?si=V3KQPSIzbeq_Pge0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

You probably dont need this, but in an event your firmware corrupts, use this to reflash your firmware.
1. Go to CircuitPython page for [XIAO RP2040](https://circuitpython.org/board/seeeduino_xiao_rp2040/), and download the `.uf2` file

    ![[Pasted image 20231013222139.png]]
2. Hold the BOOT button (located on bottom right of the controller with a letter B) then plug in USB-C cable to your XIAO RP2040 microcontroller

    ![[Pasted image 20231025090109.png]]
4. Controller will be recognize as a USB storage, place your .uf2 file inside and the controller will immediately reboot

    ![[Pasted image 20231013222113.png]]
4. Now your drive will be named CIRCUITPY and storage drop to 0.98MB and drive filled with CircuitPython libraries

    ![[Pasted image 20231013222304.png]]
    ![[Pasted image 20231013222334.png]]
## Add KMK library
You only need to do this once, and when update KMK library. 
1. Go to my [KMK github](https://github.com/4542elgh/kmk_firmware) which is a fork of official KMK with a few tweaks. Cilck on Code tab and download as ZIP

    ![[Pasted image 20231019154409.png]]
2. Extract `kmk` folder into CIRCUITPY drive root directory

    ![[Pasted image 20231013222720.png]]
## Add NeoPixel Library
Reprogram/disable NeoPixel (the white big LED in between Reset and Boot button)

![[Pasted image 20231014000633.png]]
It require a new library download into `CIRCUITPY` Drive's `lib` folder. If you do not have this folder, manually create it.
1. Download the NeoPixel library from [CircuitPython  NeoPixel](https://learn.adafruit.com/circuitpython-essentials/circuitpython-neopixel) and click on `Download Project Bundle`

    ![[Pasted image 20231015211414.png]]
2. Then open the zip file and go all the way until you enter `lib` folder

    ![[Pasted image 20231015211513.png]]
3. Copy the `neopixel.mpy` and place it inside your CircuitPython Drive's `lib` folder

    ![[Pasted image 20231015211631.png]]

# Editor
- Since this is python, you can use any text editor to edit `main.py` and `kb.py` (if your keyboard is diodeless) file, as long as it is not Office Word or any editor that add extra symbol at end of the file.
- Read–eval–print loop (REPL) is only enabled/responsive when the program does not run accordingly. If you want to force REPL, comment out `keyboard.go()` at the very end of `main.py` to stop controller automatically going into loop mode.

## MU Editor
Mu Editor is supported by [official CircuitPython documentation](https://learn.adafruit.com/welcome-to-circuitpython/installing-mu-editor), it has a good enough editor for writing python code and it will also output error messages if it encounter any. This editor auto detect COM port number and connect to REPL.
1. Download [MU Editor](https://codewith.mu/en/download)
2. Select CircuitPython on initial setup screen

    ![[Pasted image 20231013230506.png]]
3. If you need to switch to another mode, you can switch via Mode button

    ![[Pasted image 20231013230554.png]]
4. Edit `main.py` and `kb.py` (if you are working with diodeless keyboard) on the CIRCUITPY drive and everything will be recognized, including KMK library

## Putty
CircuitPython Official Doc for Putty serial port debugging: [Advanced Serial Console on Windows](https://learn.adafruit.com/welcome-to-circuitpython/advanced-serial-console-on-windows)
1. Install [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
2. You will need to connect to microcontroller via COM port. You can find COM port from device manager. 

    ![[Pasted image 20231013230059.png]]
3. Connect to REPL via Putty by using Serial mode, Speed is set to `115200`

    ![[Pasted image 20231013230222.png]]

# User configuration
If you want to work base on a clean template, there are a few in [Github](https://github.com/4542elgh/kmk_keyboard)

## KMK Diode setup (3x3 or 3x4 keyboard)

### main.py
This is where all your keymapping is
1. Create `main.py` file
2. Import classes to help you with mapping:
```python
# This is from CircuitPython, interacting with GPIO pins, also foundation of KMK
import board

# This is standard GPIO mapping for all matrix-like boards
from kmk.kmk_keyboard import KMKKeyboard

# Key Code are in this builtin class
from kmk.keys import KC
```

3. Import modules for more features
```python
# Add Media key support
# http://kmkfw.io/docs/media_keys
from kmk.extensions.media_keys import MediaKeys

# Macro
# http://kmkfw.io/docs/sequences
from kmk.handlers.sequences import simple_key_sequence

# Unicode support
# http://kmkfw.io/docs/sequences#unicode
from kmk.handlers.sequences import unicode_string_sequence

# Rotary Encoder EC11 (Knob)
# http://kmkfw.io/docs/encoder
from kmk.modules.encoder import EncoderHandler

# Hold modifier with timeout (eg. Hold alt, and switch to Layer one for knob clicking tab tab tab)
# Use KC.HM(Modifier) to place a modifier on hold until timeout
from kmk.modules.holdmod import HoldMod
```

4. Standard KMKKeyboard instance with kb.py
```python
# Initialize Keyboard class
keyboard = KMKKeyboard()
# I dont know what this means but use this value
keyboard.diode_orientation = DiodeOrientation.COL2ROW

# Define GPIO mapping
keyboard.col_pins = (board.D3, board.D2, board.D1, board.D0)
keyboard.row_pins = (board.D10, board.D6, board.D5, board.D4)
```

5. Set Unicode to Windows mode, 

Windows only: need to install [WinCompose](https://github.com/samhocevar/wincompose)
```python
# Set Unicode to Windows mode
# WINDOWS NEEDS WINCOMPOSE INSTALLED BEFORE UNICODE TO WORK
keyboard.unicode_mode = UnicodeMode.WINC
```

6. Define and enable encoder, NOTE THE TRAILING COMMAS
```python
# Regular GPIO Encoder
encoder_handler = EncoderHandler()
encoder_handler.pins = (
    # regular direction encoder and a button(define key in matrix)
    (board.D10, board.D9, None,), # encoder #1
)
```

7. Enable extension
```python
# Enable extensions
keyboard.modules = [encoder_handler, Layers(), MediaKeys()]
```

8. Define Macro (a series of keys either simultaneously, or in sequence)
```python
COPY = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.C
    )
)

CUT = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.X
    )
)

PASTE = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.V
    )
)

SCREENSHOT = simple_key_sequence(
    (
        KC.LGUI(no_release=True),
        KC.LSFT(no_release=True),
        KC.S
    )
)
```

9. Define Unicode
```python
WINK = unicode_string_sequence('😉')
SHRUG = unicode_string_sequence('¯\_(ツ)_/¯')
```

10. Add Macros to your keymap
```python
# Your Keymaps, a new line is a new row, a new list is a new layer
keyboard.keymap = [
    [KC.NO, COPY, CUT, KC.MUTE,# Each element represent a key on your keypad starting from upper left hand corner, adjust each row's length accordingly
    KC.N9, SCREENSHOT, WINK, SHRUG,
    KC.N5, KC.N6, KC.N7, KC.N8,
    KC.N1, KC.N2, KC.N3, KC.N4,
    ]
]
```

11. Reprogram/Disable NEOPIEXL LED 

    ![[Pasted image 20231014000633.png]]
```python
# Disable LED light
import neopixel
led = neopixel.NeoPixel(board.NEOPIXEL, 1)
led.brightness = 0

# Breathing Layer status LED
from kmk.extensions.statusled_neopixel import statusLED
statusLED = statusLED(
    breath_mode=True, 
    colors=(
    (255, 0, 0),
    (0, 255, 0),
    (0, 0, 255))
)
```

12. Enable keyboard and start listen to events
```python
if __name__ == '__main__':
    keyboard.go()
```

13. Save this file, and if you are in MU Editor, open REPL and see if any error is shown

### main.py complete
```python
# This is from CircuitPython, interacting with GPIO pins, also foundation of KMK
import board

# This is standard GPIO mapping for all matrix-like boards
from kmk.kmk_keyboard import KMKKeyboard

# Key Code are in this builtin class
from kmk.keys import KC

# Add Media key support
# http://kmkfw.io/docs/media_keys
from kmk.extensions.media_keys import MediaKeys

# Macro
# http://kmkfw.io/docs/sequences
from kmk.handlers.sequences import simple_key_sequence

# Unicode support
# http://kmkfw.io/docs/sequences#unicode
from kmk.handlers.sequences import unicode_string_sequence

# Rotary Encoder EC11 (Knob)
# http://kmkfw.io/docs/encoder
from kmk.modules.encoder import EncoderHandler

# Hold modifier with timeout (eg. Hold alt, and switch to Layer one for knob clicking tab tab tab)
# Use KC.HM(Modifier) to place a modifier on hold until timeout
from kmk.modules.holdmod import HoldMod

# Initialize Keyboard class
keyboard = KMKKeyboard()
# I dont know what this means but use this value
keyboard.diode_orientation = DiodeOrientation.COL2ROW

# Define GPIO mapping
keyboard.col_pins = (board.D3, board.D2, board.D1, board.D0)
keyboard.row_pins = (board.D10, board.D6, board.D5, board.D4)

# Set Unicode to Windows mode
# WINDOWS NEEDS WINCOMPOSE INSTALLED BEFORE UNICODE TO WORK
keyboard.unicode_mode = UnicodeMode.WINC

# Regular GPIO Encoder
encoder_handler = EncoderHandler()
encoder_handler.pins = (
    # regular direction encoder and a button(define key in matrix)
    (board.D10, board.D9, None,), # encoder #1
)

# Enable extensions
keyboard.modules = [encoder_handler, Layers(), MediaKeys()]

COPY = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.C
    )
)

CUT = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.X
    )
)

PASTE = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.V
    )
)

SCREENSHOT = simple_key_sequence(
    (
        KC.LGUI(no_release=True),
        KC.LSFT(no_release=True),
        KC.S
    )
)

WINK = unicode_string_sequence('😉')
SHRUG = unicode_string_sequence('¯\_(ツ)_/¯')

# Your Keymaps, a new line is a new row, a new list is a new layer
keyboard.keymap = [
    [KC.NO, COPY, CUT, KC.MUTE,# Each element represent a key on your keypad starting from upper left hand corner, adjust each row's length accordingly
    KC.N9, SCREENSHOT, WINK, SHRUG,
    KC.N5, KC.N6, KC.N7, KC.N8,
    KC.N1, KC.N2, KC.N3, KC.N4,
    ]
]

# Disable LED light
# import neopixel
# led = neopixel.NeoPixel(board.NEOPIXEL, 1)
# led.brightness = 0

# Breathing Layer status LED
from kmk.extensions.statusled_neopixel import statusLED
statusLED = statusLED(
    breath_mode=True, 
    colors=(
    (255, 0, 0),
    (0, 255, 0),
    (0, 0, 255))
)

if __name__ == '__main__':
    keyboard.go()
```

## KMK Diodeless setup (1x6 keyboard)
<div class="iframe-wrapper">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/OxEYuuUXhUQ?si=EvC0-G8DIXEQV9-X" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

### main.py
This is where all your keymapping is
1. Create `main.py` file
2. Import classes to help you with mapping:
```python
# This is from CircuitPython, interacting with GPIO pins, also foundation of KMK
import board

# CUSTOM: This is custom GPIO mapping for this board specifically
from kb import PianoKB

# Key Code are in this builtin class
from kmk.keys import KC
```

3. Import modules for more features
```python
# Add Media key support
# http://kmkfw.io/docs/media_keys
from kmk.extensions.media_keys import MediaKeys

# Macro
# http://kmkfw.io/docs/sequences
from kmk.handlers.sequences import simple_key_sequence

# Unicode support
# http://kmkfw.io/docs/sequences#unicode
from kmk.handlers.sequences import unicode_string_sequence

# Rotary Encoder EC11 (Knob)
# http://kmkfw.io/docs/encoder
from kmk.modules.encoder import EncoderHandler

# Hold modifier with timeout (eg. Hold alt, and switch to Layer one for knob clicking tab tab tab)
# Use KC.HM(Modifier) to place a modifier on hold until timeout
from kmk.modules.holdmod import HoldMod
```

4. Start A KMKKeyboard instance, either you create your own, or from existing library.

This is diodeless keyboard, you have to define your own layout and GPIO pins in `kb.py`
```python
# Initialize custom GPIO mapping kb.py
keyboard = PianoKB()
```

5. Set Unicode to Windows, Windows only: need to install [WinCompose](https://github.com/samhocevar/wincompose)
```python
# Set Unicode to Windows mode
# WINDOWS NEEDS WINCOMPOSE INSTALLED BEFORE UNICODE TO WORK
keyboard.unicode_mode = UnicodeMode.WINC
```

6. Define and enable encoder
```python
# Encoder, PINS are defined in kb.py
encoder_handler = EncoderHandler()
```

7. Enable extension
```python
# Enable extensions
keyboard.modules = [encoder_handler, Layers(), MediaKeys()]
```

8. Define Macro (a series of keys either simultaneously, or in sequence)
```python
COPY = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.C
    )
)

CUT = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.X
    )
)

PASTE = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.V
    )
)

SCREENSHOT = simple_key_sequence(
    (
        KC.LGUI(no_release=True),
        KC.LSFT(no_release=True),
        KC.S
    )
)
```

9. Define Unicode
```python
WINK = unicode_string_sequence('😉')
SHRUG = unicode_string_sequence('¯\_(ツ)_/¯')
```

10. Add Macros to your keymap
```python
# Your Keymaps, a new line is a new row, a new list is a new layer
keyboard.keymap = [
    [KC.NO, COPY, CUT, KC.MUTE,# Each element represent a key on your keypad starting from upper left hand corner, adjust each row's length accordingly
    KC.N9, SCREENSHOT, WINK, SHRUG,
    KC.N5, KC.N6, KC.N7, KC.N8, 
    KC.N1, KC.N2, KC.N3, KC.N4, 
    ]
]
```

11. Reprogram/Disable NEOPIEXL LED 

    ![[Pasted image 20231014000633.png]]
```python
# Disable LED light
import neopixel
led = neopixel.NeoPixel(board.NEOPIXEL, 1)
led.brightness = 0

# Breathing Layer status LED
from kmk.extensions.statusled_neopixel import statusLED
statusLED = statusLED(
    breath_mode=True, 
    colors=(
    (255, 0, 0),
    (0, 255, 0),
    (0, 0, 255))
)
```

12. Enable keyboard and start listen to events
```python
if __name__ == '__main__':
    keyboard.go()
```

13. Save this file, and if you are in MU Editor, open REPL and see if any error is shown

### main.py complete
```python
# This is from CircuitPython, interacting with GPIO pins, also foundation of KMK
import board

# CUSTOM: This is custom GPIO mapping for this board specifically
from kb import PianoKB

# Key Code are in this builtin class
from kmk.keys import KC

# Add Media key support
# http://kmkfw.io/docs/media_keys
from kmk.extensions.media_keys import MediaKeys

# Macro
# http://kmkfw.io/docs/sequences
from kmk.handlers.sequences import simple_key_sequence

# Unicode support
# http://kmkfw.io/docs/sequences#unicode
from kmk.handlers.sequences import unicode_string_sequence

# Rotary Encoder EC11 (Knob)
# http://kmkfw.io/docs/encoder
from kmk.modules.encoder import EncoderHandler

# Hold modifier with timeout (eg. Hold alt, and switch to Layer one for knob clicking tab tab tab)
# Use KC.HM(Modifier) to place a modifier on hold until timeout
from kmk.modules.holdmod import HoldMod

# Initialize custom GPIO mapping kb.py
keyboard = PianoKB()

# Set Unicode to Windows mode
# WINDOWS NEEDS WINCOMPOSE INSTALLED BEFORE UNICODE TO WORK
keyboard.unicode_mode = UnicodeMode.WINC

# Encoder, PINS are defined in kb.py
encoder_handler = EncoderHandler()

# Enable extensions
keyboard.modules = [encoder_handler, Layers(), MediaKeys()]

COPY = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.C
    )
)

CUT = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.X
    )
)

PASTE = simple_key_sequence(
    (
        KC.LCTRL(no_release=True),
        KC.V
    )
)

SCREENSHOT = simple_key_sequence(
    (
        KC.LGUI(no_release=True),
        KC.LSFT(no_release=True),
        KC.S
    )
)

WINK = unicode_string_sequence('😉')
SHRUG = unicode_string_sequence('¯\_(ツ)_/¯')

# Your Keymaps, a new line is a new row, a new list is a new layer
keyboard.keymap = [
    [KC.NO, COPY, CUT, KC.MUTE,# Each element represent a key on your keypad starting from upper left hand corner, adjust each row's length accordingly
    KC.N9, SCREENSHOT, WINK, SHRUG,
    KC.N5, KC.N6, KC.N7, KC.N8, 
    KC.N1, KC.N2, KC.N3, KC.N4, 
    ]
]

# Disable LED light
import neopixel
led = neopixel.NeoPixel(board.NEOPIXEL, 1)
led.brightness = 0

# Breathing Layer status LED
from kmk.extensions.statusled_neopixel import statusLED
statusLED = statusLED(
    breath_mode=True, 
    colors=(
    (255, 0, 0),
    (0, 255, 0),
    (0, 0, 255))
)

if __name__ == '__main__':
    keyboard.go()
```

### kb.py
This is where you map each switch to microcontroller pins. If you are building a custom board, take a look at [Github](https://github.com/KMKfw/kmk_firmware) zip file's `boards` folder, if a board with your controller and PCB layout has been created already. Dont re-invent the wheel.

### Custom kb.py
Since we are doing custom PCB and with a fairly new controller XIAO RP2040, we will need to define our own `kb.py` file
It is strongly recommended that you analyze the schematic of your board in order to map the correct GPIO pin
1. Create a `kb.py` file
2. Import classes to help you with mapping:
```python
# Required CircuitPython dependency
import board
# KMK base class
from kmk.kmk_keyboard import KMKKeyboard
# Direct GPIO mapping
from kmk.scanners.keypad import KeysScanner
# Rotary Encoder
from kmk.scanners.encoder import RotaryioEncoder
```

3. Define your own keyboard class, this class can be any name but need to inherit `KMKKeyboard` base class.
- This class should have a constructor and have scanners (GPIO mapping extension)
```python
class PianoKB(KMKKeyboard):
    def __init__(self):
```

4. Define your GPIO pins
- You can find GPIO pins layout from 
```python
import board
dir(board)
```

![[Pasted image 20231013231338.png]]

5. [KeysScanner](https://github.com/KMKfw/kmk_firmware/blob/master/docs/en/scanners.md#keypad-keysscanner) is direct mapping of GPIO pin, this scanner does not need diode
[RotaryioEncoder](https://github.com/KMKfw/kmk_firmware/blob/master/docs/en/scanners.md#rotaryioencoder) is also a direct mapping to GPIO pin but for Rotary Encoder
```python
# create and register the scanner
self.matrix = [
	KeysScanner(
		# require argument:
		pins=[board.D0, board.D1, board.D2, board.D3, board.D4, board.D5, board.D6],
		# optional arguments with defaults:
		# value_when_pressed=False,
		# pull=True,
		# interval=0.02,  # Debounce time in floating point seconds
		# max_events=64
	),
	RotaryioEncoder(
		pin_a=board.D10,
		pin_b=board.D9,
		# optional
		# divisor=4,
	)
]
```

6. Keymap Coordinates, this defines your keyboard physical layout
```python
KMKKeyboard.coord_mapping = [0, 1, 2, 3, 4, 5, 6, 7]
```

#### kb.py complete
This is for PianoPad since every pin is directly map to a pin on XIAO RP2040
```python
import board
from kmk.kmk_keyboard import KMKKeyboard
from kmk.scanners.keypad import KeysScanner
from kmk.scanners.encoder import RotaryioEncoder

class PianoKB(KMKKeyboard):
    def __init__(self):
        self.matrix = [
            KeysScanner(
                pins=[board.D0, board.D1, board.D2, board.D3, board.D4, board.D5, board.D6],
            ),
            RotaryioEncoder(
                pin_a=board.D10,
                pin_b=board.D9,
            )
        ]
        KMKKeyboard.coord_mapping = [0, 1, 2, 3, 4, 5, 6, 7]
```