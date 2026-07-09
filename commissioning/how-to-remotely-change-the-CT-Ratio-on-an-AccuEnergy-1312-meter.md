<div align="left"><figure><img src="../.gitbook/assets/ecosuite-logo-full.svg" alt="" width="288"><figcaption></figcaption></figure></div>

# How To remotely change the CT Ratio on an AccuEnergy 1312 meter

v2026.07.08

## Overview

This document shows you how to remotely adjust the CT ratio on an AccuEnergy 1312 meter in the case where new CTs have been installed, using an Edge Compute Node.

## Relevant Roles for this document

- Asset Manager
- Database Administrator

## Tools Needed

- Linux laptop with SolarSSH setup and Chrome or Firefox browser
- The latest sn-mbpoll utility for reading and writing to Modbus devices
- VPN software to the 4G router used
- Credentials to the Edge Compute Node you are maintaining
- Deployment details of the Edge Compute Node you are maintaining
- Credentials to the Router directly
- Modbus Map for the AccuEnergy 1312 meter
- A Hex to Decimal converter like: [https://www.rapidtables.com/convert/number/hex-to-decimal.html](https://www.rapidtables.com/convert/number/hex-to-decimal.html)

## Context

In order to update your Edge Compute Node to have the latest mbpoll version with both read and write capability you can use this command from the commandline of the SolarNode:

```
sudo apt update && sudo apt install sn-mbpoll
```

## Topology

The basic setup for this configuration is a Edge Compute Node in the field which likely connects to the internet using a 4G LTE connection maintained by a linux-booting router. The Support Laptop shown in the Figure 1 diagram below represents the field technician's computer wherever they are online with an internet connection.

<figure><img src="../.gitbook/assets/How to Debug CT.drawio (4).svg" alt=""><figcaption></figcaption></figure>

**Figure 1:** *Edge Compute Node is connected to the internet and may have multiple devices connected and CTs such as rogowski coils measuring current in a 3-Phase circuit.*

## Step-by-Step process

### Step 1: SolarSSH to the Edge Compute Node

This is a common process needed to establish secure access to the Edge Compute Node in question and any other devices on the remote subnet.

### Step 2: Review the existing CT ratio used by the Accuenergy 1312 meter

Using the command line issue this command to read register 531

```
mbpoll -a 9 -b 9600 -m rtu -t 4:hex -P none -0 -1 -v /dev/ttyUSB_1 -r 531
```

It should return a value such as:

```
[531]: 0x011F
```

Which in decimal is expressed as 287

### Step 3: Create the command that will do the register write

In a text editor, assemble the text string that you will use to write the new value to this register. You may need to express that decimal value in Hexidecimal before you create the command such as:

```
mbpoll -a 9 -b 9600 -m rtu -t 4:hex -P none -0 -1 -v -W /dev/ttyUSB_1 -r 531 0x011C
```

Which would write the value 284 in decimal to this modbus register.

### Step 4: Write the new value to the register

Paste the string into the commandline of the SolarNode and hit Enter. You should see a response from mbpoll indicating that the write was completed.

```
solar@solarnode:~$ mbpoll -a 9 -b 9600 -m rtu -t 4:hex -P none -0 -1 -v -W /dev/ttyUSB_1 -r 531 0x011C
debug enabled
iGetIntList(531)
Integer found: 531
iCount=1
Set device=/dev/ttyUSB_1
1 write data have been found
Set data=284
Word[0]=0x11C
mbpoll 1.4-26 - FieldTalk(tm) Modbus(R) Master Simulator
Copyright © 2015-2019 Pascal JEAN, https://github.com/epsilonrt/mbpoll
This program comes with ABSOLUTELY NO WARRANTY.
This is free software, and you are welcome to redistribute it
under certain conditions; type 'mbpoll -w' for details.
Opening /dev/ttyUSB_1 at 9600 bauds (N, 8, 1)
Set response timeout to 1 sec, 0 us
Protocol configuration: Modbus RTU
Slave configuration...: address = [9]
start reference = 531, count = 1
Communication......: /dev/ttyUSB_1, 9600-8N1
t/o 1.00 s, poll rate 1000 ms
Data type........: 16-bit register, output (holding) register table
[09][10][02][13][00][01][02][01][1C][E0][AA]
Waiting for a confirmation...
<09><10><02><13><00><01><F0><FC>
Written 1 references.
```

### Step 5: Check that the value has changed

This is repeating the first step which was to read the register - you should enter again:

```
mbpoll -a 9 -b 9600 -m rtu -t 4:hex -P none -0 -1 -v /dev/ttyUSB_1 -r 531
```

It should now return a value such as:

```
[531]: 0x011C
```

### Step 6: Check that the energy measurements on the Ecosuite for this node correspond to this change - that may take some time to validate.
