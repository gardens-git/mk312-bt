# Link two MK-312BT

It shouldn't be a problem to use the HC-05 Bluetooth modules to link two Boxes.

Haven't tried it, playing ideas in my head.

## Overall config

(see normal BT-conf)

```
AT // Test communication
AT+NAME=MK-312BT 
AT+UART=19200,0,0
AT+PSWD=1234
AT+POLAR=1,1
AT+IPSCAN=1024,1,1024,1 
```

## Slave config

´´´
AT+ROLE?



AT+ADDR?

```
