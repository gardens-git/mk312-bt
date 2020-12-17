# MK-312BT V1.3R

All Credits go to [qdot](https://github.com/qdot)! I'm just streamlining the project documentation here a bit for myself.

Documentation: [Readme](https://github.com/gardens-git/mk312-bt/blob/master/boms/MK-312BT_1.3R.xlsx)

Forum (for questions/discussion): https://metafetish.club ([Estim
Specific Category Here](https://metafetish.club/c/estim), but may not
include all estim messages)

## Parts ordering information

See the Documentation link above for BOMs.

Prebuilt Mouser cart for electronic components of the v1.3R board:
http://tinyurl.com/mm63csz 
- Needs updates
  - Ribbon cable to short
  - more Pins
  - Jumpers
  - Res 10K?
  - Button-Colors?
  - 
  
  
You will still need screws/standoffs etc, which can be sourced
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
- Different Design 
  - Main Boards: 2
  - Front Panel: 1
- Thickness: 1.6
- PCB Color:
  - Main Boards: No requirement, choose whatever you like
  - Front Panel: Black is recommended, but not required. Choose whatever you like otherwise.
- Surface finish: HASL
- Copper weight: 1oz
- Gold fingers: No

- Remove Order Number:
  - Front Panel: Place all Fab Markings (Serial Numbers/Date Codes) on the BOTTOM side of the front panel board or set options to no Serial / no Markings

## 3D Printable Case

STL files for a 3D printable case for the MK312 are available in the
[case/](https://github.com/gardens-git/mk312-bt/tree/master/case) directory.

## Board Assembly Instructions - IMPORTANT

### Front Panel

- Trim Potentiometer knobs to correct length if desired
- Use black permanent marker to black out front panel hole walls if
  desired
- To correctly fit LCD - solder header to LCD board first. Next
  insert LCD panel into front panel daughter board. Then put screws
  on front panel facade and finger tighten M2.5 screws first. Adjust
  positions as needed so everything lines up and sits well. Finally
  solder LCD header to front panel daughter board.
- The same goes for the buttons. 

### Main Panel

- Make sure Pin 32 (LED2) on the HC-05 module is actually soldered
  and making connection to the ZS-040 break out board or "Radio" LED
  may not function on the front panel
- Make sure to check the orientation of all electrolytic caps. (+)
  indicates the positive and a solid white "|" indicates the negative.
- **MAKE SURE YOUR POWER PLUG IS CENTER POSITIVE OR THINGS WILL GO
  BADLY.** If you fuck up and plug in a reversed polarity supply -
  replace electrolytic caps C1 and C4.
- Place transformers with P facing the output jacks (yes we are
  using the transformers "backwards" if you follow orientation given
  in the transformer datasheet )

### Case

- Use a Chisel to cut off the screw boss/screw mount on the case
  that touches/interferes with the ribbon cable socket on the
  Front Panel.

### Trouble-shooting

- Please remember to adjust the LCD contrast potentiometer - if
  incorrectly set the unit and backlight will power on but the
  interface will not be displayed.

- If Error 20 is encountered on first boot:
  - Check that all FETs and Transformers are turned in the correct direction
  - Check that resistors R35 and R46 are the correct 200k values
  - [See this thread on the message board](https://metafetish.club/t/mk-312bt-failure-20/)

## Bluetooth Serial Configuration

To do this automatically:
1. Ensure pin 32 is soldered to the carrier board (See [bluetooth/HC05PINOUT.PNG](https://github.com/buttshock/mk312-bt/blob/master/bluetooth_conf/HC05PINOUT.png) for reference)
2. Flash the included [bin file](https://github.com/gardens-git/mk312-bt/tree/master/bluetooth_conf) onto the ATMEGA16
3. Plug the HC-05 bluetooth radio into the board.
4. Place HC-05 in command mode by holding down the button on the HC-05
   module, power up MK-312BT and release button after 2 seconds. LEDs
   on HC-05 will blink slowly (0.5 Hz) to indicate command mode.
5. Watch GUI until it says the HC-05 configuration is finished and
   flash normal firmware back on to the ATMEGA16
6. If program stops on "Communicating with HC-05" make sure OSCCAL
   value for 8 MHz R/C position is copied to &H3FFF of bin location or
   use external 8 MHz crystal with appropriate fuse bits set (FFS use
   the external crystal)
   
## Firmware

1. We are using an external 8mhz crystal instead of the internal RC
   oscillator that the original uses. We need to set the fuses to
   enable this: 
   - LOW FUSE: 0xFF 
   - HIGH FUSE: 0xDC
3. Obtain or modify a copy of the firmware and flash the result onto
   the AVR (A patched version of buttshock-et312-frankenbutt-f005 is
   recommended).
