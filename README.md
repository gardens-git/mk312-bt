# MK-312BT V1.3R

All Credits go to qdot! I'm just streamlining the project here a bit for myself.

Documentation at : http://tinyurl.com/mk312bt-info

Forum (for questions/discussion): https://metafetish.club ([Estim
Specific Category Here](https://metafetish.club/c/estim), but may not
include all estim messages)

## Parts ordering information

See the Documentation link above for BOMs.

Prebuilt Mouser cart for electronic components of the v1.3R board:
https://www.mouser.com/ProjectManager/ProjectDetail.aspx?AccessID=a2725b2f11

You will still need screws/standoffs/cases etc, which can be sourced
elsewhere as listed in the spreadsheet.

## Board ordering instructions

Preferred board house is https://jlcpcb.com

You'll need 2 gerber zip files from the gerbers directory (click links below to download):

- [MK-312 V1.2 Front Panel](https://github.com/buttshock/mk312-bt/blob/master/gerbers/ZIP%20FILES/MK-312BT%20Front%20Panel%20V1.2%20Gerber.zip?raw=true)
- [MK-312 V1.3 Main Board](https://github.com/buttshock/mk312-bt/blob/master/gerbers/ZIP%20FILES/MK-312BT%20Main%20Boards%20V1.3%20Gerber.zip?raw=true)

**NOTE:** Order Front Panel and Main Board as seperate items (but they
can be in the same cart), due to difference in "Different Design"
setting. See below.

- 2 Layer PCB and Dimensions should be detected when you upload the
  zip with the gerbers
- Quantity: No requirement, choose whatever you like
- Thickness: 1.6
- PCB Color:
   - Main Boards: No requirement, choose whatever you like
   - Front Panel: Black is recommended, but not required. Choose whatever you like otherwise.
- Surface finish: HASL
- Copper weight: 1oz
- Gold fingers: No
- Material: Standard FR4
- Panel by jlcpcb: No
- Different Design 
   - Main Boards: 2
   - Front Panel: 1
- Remarks:
   - Main Boards: V Score the Main Boards panel as indicated.
   - Front Panel: Place all Fab Markings (Serial Numbers/Date Codes) on the BOTTOM side of the front panel board.
   
## Board Assembly Instructions - IMPORTANT

1. Trim Potentiometer knobs to correct length if desired
2. Use black permanent marker to black out front panel hole walls if
   desired
3. Cut the tabs off of the base of the pots so the board sits flush
   with the display facade if desired
4. To correctly fit LCD - solder header to LCD board first. Next
   insert LCD panel into front panel daughter board. Then put screws
   on front panel facade and finger tighten M2.5 screws first. Adjust
   positions as needed so everything lines up and sits well. Finally
   solder LCD header to front panel daughter board.
5. Make sure Pin 32 (LED2) on the HC-05 module is actually soldered
   and making connection to the ZS-040 break out board or "Radio" LED
   may not function on the front panel
6. Make sure to check the orientation of all electrolytic caps. (+)
   indicates the positive and a solid white "|" indicates the negative.
8. **MAKE SURE YOUR POWER PLUG IS CENTER POSITIVE OR THINGS WILL GO
   BADLY.** If you fuck up and plug in a reversed polarity supply -
   replace electrolytic caps C1 and C4.
9. Please remember to adjust the LCD contrast potentiometer - if
   incorrectly set the unit and backlight will power on but the
   interface will not be displayed.
10. Place transformers with P facing the output jacks (yes we are
    using the transformers "backwards" if you follow orientation given
    in the transformer datasheet )
11. Use a Chisel to cut off the screw boss/screw mount on the case
    that touches/interferes with the ribbon cable socket on the
    display board.
12. If Error 20 is encountered on first boot:
   - Check that all FETs and Transformers are turned in the correct direction
   - Check that resistors R35 and R46 are the correct 200k values
   - [See this thread on the message board](https://metafetish.club/t/mk-312bt-failure-20/)

## MK312-BT Firmware

1. We are using an external 8mhz crystal instead of the internal RC
   oscillator that the original uses. We need to set the fuses to
   enable this. 
   - LOW FUSE: 0xFF 
   - HIGH FUSE: 0xDC
2. A patched version of buttshock-et312-frankenbutt-f005 is
   recommended.
3. The firmware needs to be changed to replace the up/down arrows on
   the UI with left right arrows (this is because the LCD we use has a
   different character set)
4. Obtain or modify a copy of the firmware and flash the result onto
   the AVR.
5. If you don't want to use an external crystal - we need to set the
   calibration byte for the internal oscillator. If programming a
   blank chip you first need to read the calibration bytes of that
   part (i.e. using avrdude -t “dump calibration” the fourth byte is
   the calibration byte for 8MHz or use atmel studio to read the
   oscillator calibration byte). Then program that byte into 0x3fff in
   your firmware. These bytes will differ on different chips even from
   the same batch. The firmware reads that byte to set OSCCAL on
   startup. If the byte is inaccurate various problems and failures
   will occur related to timing, interrupts, and serial communication.
   Just use an external crystal dammit.

## Bluetooth Serial Configuration

Refer to
[https://www.itead.cc/wiki/Serial_Port_Bluetooth_Module_(Master/Slave)_:_HC-05](https://www.itead.cc/wiki/Serial_Port_Bluetooth_Module_(Master/Slave)_:_HC-05)
for more information.

```
// Test command to check if we are communicating (expect to receive OK)
AT

//Name the module
AT+NAME=MK-312BT 

//Set baud rate to be same as the ATMEGA16
AT+UART=19200,0,0

//Set the pairing password
AT+PSWD=1234

//Set the LED Indicator polarity (1 = High Drive 0 = Low Drive)
AT+POLAR=1,1

// Reduce power consumption by increasing the scan interval. Without
// this it consumes ~43mA when not connected and causes the 5v regulator
// to get toasty (but still within operational temp range)
AT+IPSCAN=1024,1,1024,1 
```

To do this automatically:
1. Solder Pin32 of the HC05 module (See [bluetooth/HC05PINOUT.PNG](https://github.com/buttshock/mk312-bt/blob/master/bluetooth_conf/HC05PINOUT.png) for reference)
2. Flash the included bin file onto the ATMEGA16
3. Plug the HC-05 bluetooth radio into the board (ensure pin 32 is
   soldered to the carrier board)
4. Place HC-05 in command mode by holding down the button on the HC-05
   module, power up MK-312BT and release button after 2 seconds. LEDs
   on HC-05 will blink slowly (0.5 Hz) to indicate command mode.
5. Watch GUI until it says the HC-05 configuration is finished and
   flash normal firmware back on to the ATMEGA16
6. If program stops on "Communicating with HC-05" make sure OSCCAL
   value for 8 MHz R/C position is copied to &H3FFF of bin location or
   use external 8 MHz crystal with appropriate fuse bits set (FFS use
   the external crystal)

## 3D Printable Case

STL files for a 3D printable case for the MK312 are available in the
case/ directory. For hardware, this case requires:

- 4 M2.5x20 screws
- up to 3 M2.5x5 screws
- some dampening for the battery (double sided foam tape may work)
