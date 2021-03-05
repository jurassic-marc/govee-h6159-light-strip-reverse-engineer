# Reverse Engineering Govee H6159 Wi-Fi and Bluetooth LED Light Strip

This project is concerned with reverse engineering the Govee H6159 light strips to enable Linux Bluetooth commands to be sent directly to the lights without going through the Govee Home app. 
<br>

### Disclaimer

> This project was developed using:
> + `Govee H6159 Wi-Fi and Bluetooth LED light strip`
> + `Linux`
> + `bluetoothctl`
> + `hcitools`
> + `gatttools`
<br>

The project was developed on Linux Ubuntu 16.04 LTS

> *This software was last ran on 05/03/2021.*
<br>

## Introduction

This package allows users to send basic commands to the Govee H6159 Wi-Fi and Bluetooth LED light strip through the CLI.
<br>

## Getting Started

Follow the instructions below to send commands to the light strip.
<br>

### Prerequisites

This project uses CLI commands which have been depreciated in later versions of Linux. This project assumes that these tools are installed on your local machine but it notes alternative commands that can be used when appropriate.
<br>

#### Elevate permissions to root

Run the super user command and enter your password when prompted:

```

sudo su

```
<br>

#### Ensure Bluetoothd is running

Run the system command to bring up your Bluetooth daemon service:

```

systemctl start bluetoothd

```
*or*
```

service bluetoothd start

```

To check bluetoothd is running, run the following:

```

systemctl status bluetoothd

```
*or*
```

service bluetoothd status

```

You should get a read out like:

```

● bluetooth.service - Bluetooth service
   Loaded: loaded (/etc/systemd/system/bluetooth.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-11-06 21:38:45 CET; 6s ago
     Docs: man:bluetoothd(8)
 Main PID: 2434 (bluetoothd)
   Status: "Running"
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/bluetooth.service
           └─2434 /usr/lib/bluetooth/bluetoothd

```
<br>

#### Ensure Bluetooth adaptor is up

Run the command below to bring up your registered Bluetooth adaptor:

```

hcitool dev up

```

To verify your Bluetooth adaptor is up, run this command:

```

hcitool dev

```

You should get a read out like this:

```

Devices:
        hci0    74:DF:BF:37:DA:C0

```
<br>

### Installation

To install this project on your local machine, follow these instructions:

1. Clone this repo to your local machine:
```

git clone <HTTPS URL>/govee-h6159-light-strip-reverse-engineer.git /path/to/proj/

```
<br>

2. Turn on your H6159 light strip.
<br>


### Configuration

To configure this application follow these steps.
<br>


##### Discover your H6159 light strip's MAC address
___

In a Linux terminal run the following commands:

```

bluetoothctl

```
*then in the Bluetooth controller prompt*
```

scan on

```
*then after a few seconds*
```

scan off

```
*finally, search through the printed MAC addresses for the light strip's, it should look something like this*
```

[NEW] Device A4:C1:38:F5:02:DB ihoment_H6159_51324

```
*or*
```

hcitool scan

```
*then search through the printed MAC addresses for the light strip's, it should look something like this*
```

[NEW] Device A4:C1:38:F5:02:DB ihoment_H6159_51324

```

> **Note:** If the light strip was undetected, run the above command with lescan rather than scan.
<br>

##### Verify your H6159 light strip

This is an optional step to verify that our MAC address is correct by running the following:

```

hcitool info A4:C1:38:F5:02:DB

```

You should get something similar to the following:

```

Requesting information ...
        Handle: 32 (0x0020)
        LMP Version: 4.2 (0x8) LMP Subversion: 0x22bb
        Manufacturer: Telink Semiconductor Co. Ltd (529)
        Features: 0x39 0x00 0x00 0x00 0x00 0x00 0x00 0x00

```
<br>

##### Alter the script to point to your light's MAC address

In the scripts change the placeholder MAC address with your light's:

```

gatttool -b A4:C1:38:F5:02:DB --char-write-req --handle 0x0015 --value 3305020000FF00000000000000000000000000CB > /dev/null

```
<br>

### Operation

Follow the steps below to operate your lights.
<br>

#### Turn the lights on or off

Run the corresponding script:
```

sh ./bin/turn_lights_[on|off].sh
```
<br>

#### Change the lights colour to red, green or blue

Run the corresponding script:
```

sh ./bin/change_lights_colour_to_[red|green|blue].sh

```
<br>

## Issues

#### No Bluetooth adaptors detected

If running the following command:

```

hcitool dev

```

Results in:

```

Devices:
        _____


```

Your Linux OS does not recongise any Bluetooth adaptors. 
This is likely if you are running Linux non-natively, i.e. Virtual Machine, virtualisation, etc.
Install Linux natively as a dual-boot OS or seek online advice.
<br>

## Author

[Marc Templeton](https://github.com/jurassic-marc) | [LinkedIn](https://www.linkedin.com/in/marc-templeton/) | [Medium](https://medium.com/@marctempleton)