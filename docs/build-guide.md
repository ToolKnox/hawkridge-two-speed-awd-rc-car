# 3D-Printed 2-Speed AWD RC Car with ESP32 Remote

https://github.com/ToolKnox/hawkridge-two-speed-awd-rc-car

Remix of Crawler V5 by TRDB (Instructables, CC BY 4.0), updated/refurbished for modern use.

## What you are building

A roughly 47 x 39 cm, almost 7 kg all-wheel-drive car printed in PETG and TPU. It has a 775 brushed motor, a
two-speed dog-clutch gearbox, a magnetic clutch, an asymmetric centre differential with a lock, Torsen front and
rear differentials, a stepper-driven parking brake, rack-and-pinion steering and independent suspension with double
Cardan joints. The car runs three ESP32 boards: a main controller and a brake controller in the car, plus one in the
remote control. The car and remote talk over ESP-NOW. You do every calibration from the remote's LCD menu, so you
never have to edit code.

This is a long build: 243 printed parts, two custom PCBs and a lot of cut-to-length metal stock. The original
author put the full parts cost at more than 1,100 EUR when buying everything new.

## Files in this package

- 243 STL files, named `SS-NN-QQ name.stl`. `SS` is the build section, `NN` the part number and `QQ` how many to
  print. Print every part whose name contains `TPU` in TPU and everything else in PETG.
- `STEP-01` to `STEP-12` zips: the original editable STEP files, one zip per build section.
- Section 12 (high-speed option) is optional. It replaces part 03-09 and part 04-02 with 30-tooth versions.
- Bill of Material PDF with shop links, and this guide.
- Sketches and PCB Gerbers are in the GitHub repository above (`CrawlerV5main`, `CrawlerV5aux`, `CrawlerV5rc`,
  `Car PCB files`, `RC PCB files`).

## 1. Install and flash first

1. Install Arduino IDE. In Boards Manager install **esp32 by Espressif version 2.0.17**. The sketches do not compile
   on core 3.x.
2. In Library Manager install the libraries the sketches include:
   - car main: **Adafruit MCP23017 Arduino Library** (`Adafruit_MCP23X17.h`), **Adafruit ADS1X15**,
     **Adafruit NeoPixel**, **Adafruit PWM Servo Driver Library**
   - remote: **Adafruit ADS1X15** and **LiquidCrystal I2C** (`LiquidCrystal_I2C.h`)
   - car auxiliary: nothing extra.

   WiFi, esp_now, esp_wifi, Wire and Preferences come with the ESP32 core.
3. Flash the boards before you plug them into the PCBs:
   - car main ESP32 (WROOM-32U, external antenna): `CrawlerV5main.ino`
   - car auxiliary ESP32 (onboard antenna is fine, it does not use radio): `CrawlerV5aux.ino`
   - remote ESP32 (WROOM-32U): `CrawlerV5rc.ino`
   - For the high-speed option, flash `CrawlerV5main_highspeed` and `CrawlerV5rc_highspeed` instead.
4. The sketches set fixed custom MAC addresses for the car and the remote, so pairing needs no edits. Change both
   only if you run two cars at the same time. All three sketches print debug output at 115200 baud.

## 2. Order the PCBs and named parts

- **Car PCB V1.1** and **RC PCB**: send the Gerber zips from the repository to any 2-layer PCB fab. The original
  boards were made by AISLER.
- Buy these parts by part number from an electronics distributor. The PCB footprints need these exact parts:
  - Molex SL crimp pins 16-02-0102 (female) and 16-02-0114 (male)
  - Molex SL board sockets 70543-0001/-0002/-0003 (2/3/4-pin)
  - Molex SL housings 50-57-9402/-9403/-9404 and 70107-0001/-0002
  - Gaptec LC78_05-2.0 (5 V 2 A), 2x Gaptec LME78_03-1.0 (3.3 V) and 1x LME78_05-1.0 (5 V 1 A) DC-DC converters
  - AKL 175-04 terminal block
  - IMAXX H7810 miniOTO fuse holder
  - 4x 10 uF 50 V X7R capacitors in 2220
  - A crimp tool for Molex SL. The author uses an Engineer PA-09.
  - 2x Marquardt 1821.1101 toggle switches and 2x Marquardt 1801.6115 rocker switches (one for the car, one for
    the remote). The printed panels are cut for these switch bodies.
  - 1x Panasonic AV32543AT roller-lever micro switch (parking-brake sensor).
- 8x Febrotec 0C0480-0512500M compression springs for the shock absorbers, from Febrotec.
- Everything else is in the Bill of Material.

## 3. Print

- The tolerances (tight and loose fits) were set for the default PETG and TPU profiles of a Prusa i3 MK3S. On
  another printer, start from its default profile and print one bearing seat and one insert hole as a test first.
- Print round bearing seats with their axis vertical so they come out round. Only use supports where an overhang
  exceeds 45 degrees. Print the brake worm gear at 45 degrees with support.
- Holes for heat-set inserts are 4.1 mm (M3 inserts). Hex pockets take M3 nuts.
- The author bought 4 kg of PETG and 1 kg of TPU (Recreus FilaFlex 40, 2x 500 g) for one car.

## 4. Prepare metal parts

Cut the 3 mm steel bar, 6 mm aluminium bar and 4 mm steel bar to these lengths, then deburr:

| Qty | Part | Used in |
|---|---|---|
| 2 | 3 mm steel, 81 mm | gearbox shaft |
| 2 | 6 mm aluminium, 70 mm | gear-shift slider rails (LM6UU) |
| 1 | 3 mm steel, 65 mm | clutch output shaft |
| 2 | 6 mm aluminium, 73 mm | clutch slider |
| 1 | 3 mm steel, 47 mm | centre differential |
| 1 | 3 mm steel, 37 mm | rotation inverter |
| 2 | 6 mm aluminium, 46 mm | differential lock |
| 1 | 4 mm steel, 24 mm | brake |
| 12 | 3 mm steel, 33 mm | front and rear differentials |
| 2 | 6 mm aluminium, 124 mm | steering |
| 4 | 3 mm steel, 18 mm | tie-rod joints |
| 1 | 3 mm steel, 6 mm | top of servo horn |
| 8 | 6 mm aluminium, 70 mm | shock absorbers (see below) |
| 4 | 3 mm steel, 49 mm | shock absorber joints |
| 4 | 3 mm steel, 18 mm | front wishbone joint adapters |
| 2 | 4 mm steel, 23 mm | joystick levers |
| 2 | M4 threaded rod, 390 mm | base frame |

For each shock absorber bar, tap an M3 thread into one end. Drill a 3 mm cross hole 4 mm from the other end.

Many shaft assemblies are locked axially with short pieces of 1.5 mm2 solid copper wire pushed through matching
holes in the mating printed parts. This joint does not loosen, and you can still take it apart.

## 5. Assemble the mechanics

Work through the sections in file-number order.

1. **Main body (01).** Build the base frame and fit the two M4 rods with two nuts per side, locked against each other.
   Fit 683 bearings in the shock-absorber joints. Cut two 15-LED pieces from the WS2812B strip, solder leads and push
   them into the LED bars: front LEDs 0-14, rear 15-29, in series. Wire the four E10 headlights in parallel. Leave the
   front plate (01-09), the cage, and the top and bottom covers off for now.
2. **Engine and gearbox (02).** Mount the 775 motor with 2x M4x10. Replace the coupler's grub screws with M3 socket
   screws. The motor gears are linked through a TPU part to damp vibration. The slider runs on LM6UU bearings on the
   6 mm rails. Fix the bearings in gears 14 and 15 by fusing the lock rings to the gears with a soldering iron at
   about 240 degrees C.
3. **Clutch (03).** Fix six 8x3 mm countersunk ring magnets with M3 countersunk screws on each disc: one disc north
   faces out, the other south. Fit the 6804 bearing loosely in the slider, as designed.
4. **Centre differential (04).** It splits torque 55 % rear / 45 % front and has a dog-clutch lock and a rotation
   inverter. Pins are 3 mm steel; the lock slider runs on 6 mm aluminium.
5. **Actuators (05).** Build four: two of version I and two of version II. Press each N20 gear on straight, using a
   vise. Cut each PC20BU potentiometer shaft to length and grind it to a double-D for a tight fit. Fit the
   potentiometer at its centre position with the driven slider at mid-travel.
6. **Brake (06).** Fit the NEMA 14 stepper, the AV32543 roller switch (brake sensor) and the 4 mm x 24 mm shaft.
7. **Front and rear differentials (07).** These are Torsen-style. Use twelve 3 mm x 33 mm pins.
8. **Steering (08).** Rack and pinion driven by the SF3218MG servo and its horn, on 6 mm aluminium guides.
9. **Wheels and suspension (09).** Double Cardan drive shafts, wishbones, printed shock absorbers with the Febrotec
   springs and an M3 washer on top. Tyres are TPU.
10. Fit the **cage** (parts 01-43 to 01-45) before you run any actuator. Without it the base frame flexes and gears
    can skip.

## 6. Electronics and wiring

1. Populate the car PCB:
   - female header strips for the 2 ESP32, 2 ADS1115, 4 DRV8876 (the QFN carrier, Pololu item 4036) and 1 DRV8825.
     38-pin ESP32 boards come in two row spacings; check that yours matches the header positions on the PCB.
   - MCP23017 in a 28-pin socket
   - the DC-DC converters, capacitors, resistors per the silkscreen, 30 A fuse and holder, IRFP7430 power switch
     MOSFET, IRLZ44N headlight MOSFET, three brake buttons, the green LED and the Molex SL sockets
   - The optional I2C pull-up and PWM pull-down positions can stay empty.
2. Mount the PCB and the MD20A on their printed frames with M3 screws. Fix the ACS712 with two cable ties. The
   PCA9685 clamps between brackets.
3. Power path: battery, XT60, then the car PCB. Route one battery lead through the ACS712 current sensor. Use 12 AWG
   silicone wire for the battery and the 775 motor. Solder high-current wires to the ACS712 pins, not its screw
   terminal.
4. Connect only the steering servo **signal** wire to PCA9685 channel 0. The servo's power comes from the car PCB.
5. Use 24 AWG wire with Molex SL connectors for sensors, actuators and LEDs. Never plug an actuator motor in without
   its potentiometer connected.
6. Remote control:
   - Populate the RC PCB: ESP32-32U, ADS1115, level shifter, 3.3 V and 5 V converters.
   - Wire the 20x4 I2C LCD, two Bourns joystick potentiometers, the PC20BU speed knob, the on/off switch, two toggle
     switches and four LED push buttons.
   - Power comes from the 6x AA holder (NiMH cells).

## 7. First power-up

1. For the first power-up, disconnect every motor. Hold the car so nothing can move.
2. Switch on the remote, then the car. With no link, the car LEDs blink red. Once paired, they show a running light
   and the remote shows the main screen.
3. On the remote main screen, check the battery voltage and current readings. Then connect the motors one at a time.

## 8. Calibrate (remote menu, chassis open)

Remove the bottom cover so you can see the actuators. Grease all drivetrain gears with plastic-safe PTFE grease now:
greasing is mandatory. Keep a finger near the power switch the whole time.

1. **Joysticks:** move each stick to its limits when asked. Leave the dead-band at the default.
2. **Steering:** set the left and right limits, then the centre. Values run from 150 to 600 servo counts. Do not let
   the tie rods touch the fenders.
3. **Gear shift, clutch, differential lock:**
   - Step each actuator with the left buttons and confirm each end position with the upper right button.
   - Move clutch actuators A and B alternately with the left toggle switch so the slider never tilts.
   - Stop each actuator just before the hard end stop.
4. **Parking brake** (buttons on the car PCB):
   - The LED blinks at 1 Hz while the brake is not calibrated. Press the middle button to start; it blinks at 2 Hz.
   - Hold the left or right button to drive the brake towards the sensor until it triggers.
   - Close the brake (LED blinks at 5 Hz) until you can no longer turn the drive shaft by hand. Do not go further:
     the brake can jam.
   - Press the middle button again.
   - In use, the LED blinks every 4 seconds with the brake open, flashes fast while it moves and stays on when it
     is closed.
   - Set the DRV8825 current limit low. About 0.5 A total on the remote display while the brake moves is enough.
   - If the stepper only buzzes, swap its coil wires. Each coil pair measures under 100 ohm.
5. **Headlight brightness and RGB colour:** set these at the end of the menu.

## 9. Driving

| Control | Function |
|---|---|
| Right joystick | throttle and direction |
| Left joystick | steering |
| Knob | maximum speed, 20-100 % PWM |
| Left toggle | clutch |
| Right toggle | gear |
| Upper left button | parking brake |
| Lower left button | differential lock |
| Upper right button | headlights |

- Lock the differential only with the clutch open or a wheel free, so the dog teeth do not jam.
- Shifting with the clutch closed opens the clutch automatically, shifts, then closes it again.
- Low car battery: when the 3S pack keeps reading below 11.1 V at idle (under 2 A) or below 10.8 V under load, the
  remote stops sending drive, brake, differential-lock and headlight commands. Charge the pack. Never discharge a
  3S pack below 9.9 V or charge it above 12.6 V.
- The remote LCD shows `Bt low` instead of its own voltage when the NiMH pack drops below 6.8 V.
- Top speed is about 11.7 km/h in the fast gear and 6.5 km/h in the slow gear.

Credits: original design, code and PCBs by TRDB (Thomas Böhm), Crawler V5,
https://www.instructables.com/3D-Printed-Remote-Controlled-2-speed-AWD-Car/ and https://github.com/TRD-B/Crawler-V5,
licensed CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/). This guide is a rewritten, reorganised version;
the print files are unchanged.
