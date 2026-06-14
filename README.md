# MorseDeck Terminal

> A standalone Morse code keyer and trainer built on the ESP32-S3, featuring real-time decoding, four keying modes, a rotary settings menu, and T9-style keyboard replay.

## Full Feature Demonstration & Walkthrough

<p align="center">
  <a href="https://youtu.be/P0Hb8axpHqs">
    <img src="https://img.youtube.com/vi/P0Hb8axpHqs/0.jpg" width="400" alt="Watch demo">
  </a>
</p>


## Table of Contents

- [Overview](#overview)
- [Functionality](#functionality)
  - [Operating Modes](#operating-modes)
  - [Keying Modes](#keying-modes)
  - [Morse Decoding](#morse-decoding)
  - [Keyboard Replay Mode](#keyboard-replay-mode)
  - [Settings Menu](#settings-menu)
- [System Design](#system-design)
  - [Hardware](#hardware)
  - [PCB Design](#pcb-design)
  - [Software](#software)
  - [Pin Mapping](#pin-mapping)
- [Bill of Materials](#bill-of-materials)
- [Author](#author)
- [License](#license)


## Overview

MorseDeck Terminal is a fully self-contained Morse code keyer and trainer built on the ESP32-S3 and programmed in MicroPython. The device supports professional-grade iambic keying with automatic real-time decoding, a rotary encoder settings interface, multi-tap keyboard input, and serial output to an external display, all running on a custom PCB in a 3D-printed enclosure.

The project was designed and built from scratch for **EENG 163: Introduction to Embedded Systems** at Eastern Washington University, covering real-time signal generation, hardware input handling, OLED display rendering, and inter-device communication.


## Functionality

### Operating Modes

The device ships with five selectable operating modes, accessible from the settings menu:

| Mode | Description |
|---|---|
| **Sandbox** | Free-form keying with live decoding on the OLED |
| **Keyboard Replay** | Type text via keypad and play it back as Morse |
| **Debug** | Live readout of paddle state, encoder pins, keypad input, and memory |

### Keying Modes

Four keying styles are supported, covering both traditional and modern paddle techniques:

| Mode | Behavior |
|---|---|
| **Iambic A** | Alternates dit/dah while both paddles are held; stops on release |
| **Iambic B** | Same as A, but sends one extra element after both paddles are released |
| **Ultimatic** | Last-pressed paddle wins when both are held simultaneously |
| **Straight Key** | Either paddle held produces a continuous tone |

### Morse Decoding

Gap detection runs automatically in the background and scales with the live WPM setting:

- After **3x the dit duration** of paddle idle time, a letter space is inserted and the buffered sequence is decoded to the OLED
- After **7x the dit duration**, the space is upgraded to a word break

### Keyboard Replay Mode

Text can be entered using the 4x4 keypad with T9-style multi-tap input, following a standard phone keyboard layout. The device encodes the typed text to Morse in real time, displays it on the OLED, and plays it back through the buzzer on command.

| Key | Action |
|---|---|
| A | Backspace |
| B | Play current text as Morse |

### Settings Menu

Hold the rotary encoder button for **2 seconds** to open the settings menu. Short-press to enter edit mode on any item, then rotate to adjust.

| Setting | Options |
|---|---|
| Mode | Sandbox, Keyboard Replay, Debug |
| Keying Mode | Iambic A, Iambic B, Ultimatic, Straight Key |
| WPM | Adjustable (scales all timing thresholds) |
| Volume | 0-10 |
| LED | Enabled / Disabled |
| Decoding | Enabled / Disabled |
| About | Scrolling credits page |
| Reset | Hard resets the device |

Additional behaviors include a typewriter-style boot screen on startup and a bouncing-logo screensaver after 60 seconds of inactivity.


## System Design

### Hardware

| Component | Details |
|---|---|
| Microcontroller | ESP32-S3 |
| Display | SSD1306 128x64 OLED (SoftI2C) |
| Paddles | 2x momentary buttons (Dit / Dah) |
| Keypad | 4x4 membrane matrix keypad |
| Rotary Encoder | CLK, DT, SW with long-press menu activation |
| Buzzer | Piezo PWM buzzer |
| LED | Single external indicator LED |

### PCB Design

Designed in KiCad. All project files are available in [`/PCB Design - KiCad Program/ESP32 Board`](PCB%20Design%20-%20KiCad%20Program/ESP32%20Board).

<img src="https://raw.githubusercontent.com/SethHibpshman/MorseDeck-Terminal/main/assets/pcb_layout.png" width="30%">

### Software

Written entirely in **MicroPython** as a single-file application.

**Dependencies:**
- `ssd1306` - OLED driver
- `gfx` - OLED graphics primitives
- `keypad` - 4x4 matrix keypad driver
- `urandom` - screensaver randomization

```
main.py    # Full application: hardware init, all functions, main loop
```

### Pin Mapping

| Pin | Function |
|---|---|
| 7 | Dit paddle |
| 16 | Dah paddle |
| 38 | Buzzer (PWM) |
| 1 | External LED |
| 4 | Rotary CLK |
| 47 | Rotary DT |
| 45 | Rotary SW |
| 46 | OLED SCL |
| 8 | OLED SDA |
| 5, 6, 15, 17 | Keypad rows |
| 18, 2, 39, 40 | Keypad columns |


## Bill of Materials

| Item | Qty | Unit | Total |
|---|---|---|---|
| [ESP32-S3 Dev Board](https://a.co/d/08OKpdB2) | 1 | $6.40 | $6.40 |
| SSD1306 OLED 128x64 | 1 | $3.00 | $3.00 |
| Rotary Encoder | 1 | $3.00 | $3.00 |
| 4x4 Membrane Keypad | 1 | $1.60 | $1.60 |
| PCBs | 1 | - | $5.00 |
| Piezo Buzzer (Active) | 1 | $0.35 | $0.35 |
| Resistors | - | - | $0.12 |
| LED | 1 | $0.03 | $0.03 |
| 3D-printed enclosure | 1 | - | $0.00 |
| **Grand Total** | | | **$19.50** |

> **Notes:**
> - Unit costs reflect bulk purchasing; individual retail prices may be higher.
> - Some components were salvaged or borrowed; actual out-of-pocket cost may be lower.
> - 3D printing assumes university or personal printer access. Outsourced printing would add to the total.
> - PCB cost assumes overseas fabrication. Each revision spin adds ~$15 per run.


## Author

**Seth Hibpshman**  
Student of Electrical Engineering, Eastern Washington University


## License

[GNU General Public License v3.0](LICENSE)
