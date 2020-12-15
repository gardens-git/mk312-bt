# AT COMMANDS

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
1. Ensure pin 32 is soldered to the carrier board (See [bluetooth/HC05PINOUT.PNG](https://github.com/buttshock/mk312-bt/blob/master/bluetooth_conf/HC05PINOUT.png) for reference)
2. Flash the included bin file onto the ATMEGA16
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
