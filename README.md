![GitHub Logo](https://github.com/gnea/gnea-Media/blob/master/Grbl%20Logo/Grbl%20Logo%20250px.png?raw=true)

***
Ce fork me permet de sauvegarder le paramétrage et la configuration d'un _Drawing Robot_ personnalisé, basé sur le [Drawing Robot by henryarnold](https://www.thingiverse.com/thing:2349232).

Les fichiers GRBL qui diffèrent du dépôt d'origine sont :
 * `config.h` ;
 * `defaults.h` ;
 * `spindle_control.c` (copié depuis le repo [Grbl with Pen Servo](https://github.com/bdring/Grbl_Pen_Servo/blob/master/grbl/spindle_control.c) pour faire fonctionner le servo).

Pilotage du servo :
```
M03 S255 // abaisser le stylo
M03 S0   // remonter le stylo
```

Ce dépôt contient également les fichiers à imprimer en 3D issus de :
 * [Drawing Robot - Arduino Uno + CNC Shield + GRBL](https://www.thingiverse.com/thing:2349232)
 * [Idler Pulley for 624 Bearing and GT2 belt](https://www.thingiverse.com/thing:2424284)

J'utilise [UGS](https://winder.github.io/ugs_website/) pour les réglages et [LaserWeb (version 4.0)](https://laserweb.yurl.ch/) pour lancer le tracer de dessins.

Après quelques essais, la configuration de GRBL retenue est la suivante :

```
Grbl 1.1g ['$' for help]
$0=10
$1=25
$2=0
$3=0
$4=0
$5=0
$6=0
$10=1
$11=0.010
$12=0.002
$13=0
$20=0
$21=0
$22=1
$23=3
$24=25.000
$25=500.000
$26=250
$27=1.000
$30=1000
$31=0
$32=0
$100=80.000
$101=80.000
$102=250.000
$110=2000.000
$111=2000.000
$112=500.000
$120=50.000
$121=50.000
$122=10.000
$130=250.000
$131=225.000
$132=200.000
```

***
_Click the `Release` tab to download pre-compiled `.hex` files or just [click here](https://github.com/gnea/grbl/releases)_
***
Grbl is a no-compromise, high performance, low cost alternative to parallel-port-based motion control for CNC milling. This version of Grbl runs on an Arduino with a 328p processor (Uno, Duemilanove, Nano, Micro, etc).

The controller is written in highly optimized C utilizing every clever feature of the AVR-chips to achieve precise timing and asynchronous operation. It is able to maintain up to 30kHz of stable, jitter free control pulses.

It accepts standards-compliant g-code and has been tested with the output of several CAM tools with no problems. Arcs, circles and helical motion are fully supported, as well as, all other primary g-code commands. Macro functions, variables, and most canned cycles are not supported, but we think GUIs can do a much better job at translating them into straight g-code anyhow.

Grbl includes full acceleration management with look ahead. That means the controller will look up to 16 motions into the future and plan its velocities ahead to deliver smooth acceleration and jerk-free cornering.

* [Licensing](https://github.com/gnea/grbl/wiki/Licensing): Grbl is free software, released under the GPLv3 license.

* For more information and help, check out our **[Wiki pages!](https://github.com/gnea/grbl/wiki)** If you find that the information is out-dated, please to help us keep it updated by editing it or notifying our community! Thanks!

* Lead Developer: Sungeun "Sonny" Jeon, Ph.D. (USA) aka @chamnit

* Built on the wonderful Grbl v0.6 (2011) firmware written by Simen Svale Skogsrud (Norway).

***

### Official Supporters of the Grbl CNC Project
![Official Supporters](https://github.com/gnea/gnea-Media/blob/master/Contributors.png?raw=true)


***

## Update Summary for v1.1
- **IMPORTANT:** Your EEPROM will be wiped and restored with new settings. This is due to the addition of two new spindle speed '$' settings.

- **Real-time Overrides** : Alters the machine running state immediately with feed, rapid, spindle speed, spindle stop, and coolant toggle controls. This awesome new feature is common only on industrial machines, often used to optimize speeds and feeds while a job is running. Most hobby CNC's try to mimic this behavior, but usually have large amounts of lag. Grbl executes overrides in realtime and within tens of milliseconds.

- **Jogging Mode** : The new jogging commands are independent of the g-code parser, so that the parser state doesn't get altered and cause a potential crash if not restored properly. Documentation is included on how this works and how it can be used to control your machine via a joystick or rotary dial with a low-latency, satisfying response.

- **Laser Mode** : The new "laser" mode will cause Grbl to move continuously through consecutive G1, G2, and G3 commands with spindle speed changes. When "laser" mode is disabled, Grbl will instead come to a stop to ensure a spindle comes up to speed properly. Spindle speed overrides also work with laser mode so you can tweak the laser power, if you need to during the job. Switch between "laser" mode and "normal" mode via a `$` setting.

	- **Dynamic Laser Power Scaling with Speed** : If your machine has low accelerations, Grbl will automagically scale the laser power based on how fast Grbl is traveling, so you won't have burnt corners when your CNC has to make a turn! Enabled by the `M4` spindle CCW command when laser mode is enabled!

- **Sleep Mode** : Grbl may now be put to "sleep" via a `$SLP` command. This will disable everything, including the stepper drivers. Nice to have when you are leaving your machine unattended and want to power down everything automatically. Only a reset exits the sleep state.

- **Significant Interface Improvements**: Tweaked to increase overall performance, include lots more real-time data, and to simplify maintaining and writing GUIs. Based on direct feedback from multiple GUI developers and bench performance testing. _NOTE: GUIs need to specifically update their code to be compatible with v1.1 and later._

	- **New Status Reports**: To account for the additional override data, status reports have been tweaked to cram more data into it, while still being smaller than before. Documentation is included, outlining how it has been changed.
	- **Improved Error/Alarm Feedback** : All Grbl error and alarm messages have been changed to providing a code. Each code is associated with a specific problem, so users will know exactly what is wrong without having to guess. Documentation and an easy to parse CSV is included in the repo.
	- **Extended-ASCII realtime commands** : All overrides and future real-time commands are defined in the extended-ASCII character space. Unfortunately not easily type-able on a keyboard, but helps prevent accidental commands from a g-code file having these characters and gives lots of space for future expansion.
	- **Message Prefixes** : Every message type from Grbl has a unique prefix to help GUIs immediately determine what the message is and parse it accordingly without having to know context. The prior interface had several instances of GUIs having to figure out the meaning of a message, which made everything more complicated than it needed to be.

- New OEM specific features, such as safety door parking, single configuration file build option, EEPROM restrictions and restoring controls, and storing product data information.

- New safety door parking motion as a compile-option. Grbl will retract, disable the spindle/coolant, and park near Z max. When resumed, it will perform these task in reverse order and continue the program. Highly configurable, even to add more than one parking motion. See config.h for details.

- New '$' Grbl settings for max and min spindle rpm. Allows for tweaking the PWM output to more closely match true spindle rpm. When max rpm is set to zero or less than min rpm, the PWM pin D11 will act like a simple enable on/off output.

- Updated G28 and G30 behavior from NIST to LinuxCNC g-code description. In short, if a intermediate motion is specified, only the axes specified will move to the stored coordinates, not all axes as before.

- Lots of minor bug fixes and refactoring to make the code more efficient and flexible.

- **NOTE:** Arduino Mega2560 support has been moved to an active, official Grbl-Mega [project](http://www.github.com/gnea/grbl-Mega/). All new developments here and there will be synced when it makes sense to.


```
List of Supported G-Codes in Grbl v1.1:
  - Non-Modal Commands: G4, G10L2, G10L20, G28, G30, G28.1, G30.1, G53, G92, G92.1
  - Motion Modes: G0, G1, G2, G3, G38.2, G38.3, G38.4, G38.5, G80
  - Feed Rate Modes: G93, G94
  - Unit Modes: G20, G21
  - Distance Modes: G90, G91
  - Arc IJK Distance Modes: G91.1
  - Plane Select Modes: G17, G18, G19
  - Tool Length Offset Modes: G43.1, G49
  - Cutter Compensation Modes: G40
  - Coordinate System Modes: G54, G55, G56, G57, G58, G59
  - Control Modes: G61
  - Program Flow: M0, M1, M2, M30*
  - Coolant Control: M7*, M8, M9
  - Spindle Control: M3, M4, M5
  - Valid Non-Command Words: F, I, J, K, L, N, P, R, S, T, X, Y, Z
```

-------------
Grbl is an open-source project and fueled by the free-time of our intrepid administrators and altruistic users. If you'd like to donate, all proceeds will be used to help fund supporting hardware and testing equipment. Thank you!

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CUGXJHXA36BYW)
