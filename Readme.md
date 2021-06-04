# sardana-KepcoMotorControllerSerialGPIB
This a Sardana MotorController for controlling Kepco BOP power supplies using Serial-to-GPIB-adapters. It uses the standard command strings needed for controlling a Kepco GPIB power supply, but sends them to a serial ttyDevice instead of a real GPIB port. It thus allows to convert a GPIB-controllable Kepco to a serial device.

# Installation
## USB Serial converter
Create a udev rule in order to mount the USB Serial converter always under the same link, e.g. ```/dev/ttyKepco```.

First check the VendorID, ProductID, and SerialNumber using ```dmesg```. Then add a new udev rule:
```
sudo nano /etc/udev/rules.d/55-usbcom.rules
SUBSYSTEM=="tty", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", ATTRS{serial}=="PX4UALD3", SYMLINK+="ttyKepco", MODE="0666"
```
Reload and apply the udev rule by
```
sudo udevadm control --reload
sudo udevadm trigger --action=add
```

## Serial-to-GPIB adapter
The Sardana controller does not allow to configure parameters specific to the GPIB protocol. This has to be done before in the respective Serial-to-GPIB adapter, which usually allow to set the GPIB address and other parameters via serial control strings or the a configuration utility provided by the manufacturer. Don't forget to save the parameters in the adapters non-volatile memory.

## Sardana Configuration
The ```KepcoMotorControllerSerialGPIB``` Class requires to set the following properties:
- Port (e.g., ```/dev/ttyKepco```)

## Authors
Martin Hennecke
