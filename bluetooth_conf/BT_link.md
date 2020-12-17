# Link two MK-312BT

It shouldn't be a problem to use the HC-05 Bluetooth modules to link two Boxes.

At the end we will get one box exactly "normal", waiting for BT connections for remote commands and one trying to connect to the other from start up. The paring on the firmware level shoud work from both boxes.

Haven't tried it, playing ideas in my head.

## Overall config

(see normal BT-conf, same for master and slave, maybe change to different names)

```
AT // Test communication
AT+NAME=MK-312BT 
AT+UART=19200,0,0
AT+PSWD=1234
AT+POLAR=1,1
AT+IPSCAN=1024,1,1024,1 
```

## Slave config
```
AT+ROLE? // check the role, must be "0"
AT+ADDR? // check the adress, we will need it for the master, will lock like xxxx:yy:zzzzzz
```
## Master config
```
AT+ROLE=1 // Set module to Bluetooth master mode
AT+CMODE=0 // This will set the connect mode to “fixed address” 
AT+BIND=xxxx,yy,zzzzzz // This will set the fixed address from the slave  (be shure to use , not : here)
```
