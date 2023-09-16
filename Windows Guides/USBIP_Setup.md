# Connecting USB Devices to WSL Through usbipd-win

## Introduction

While using WSL for crazyflie/swarm development has many advantages over VM's, one downside is that WSL does not (currently) natively support connecting usb devices plugged into your Windows computer to your Linux distribution in WSL. However, [usbipd-win](https://github.com/dorssel/usbipd-win) is an open-source project that will allow us to do just that. 

>Note: While usbipd-win can connect crazyradio's without problem to WSL for teleoperation, not all usb devices may succeed in being connected. In particular, we have found that we cannot get a physical wired usb connection from the crazyflie to the computer to work through usbipd-win.

This tutorial borrows from [Microsoft's Connect USB Devices Page](https://learn.microsoft.com/en-us/windows/wsl/connect-usb) and of course, [the WSL documentation for usbipd-win](https://github.com/dorssel/usbipd-win/wiki/WSL-support). 

## Step 1: Ensure Prerequisites

According to [Microsoft's Connect USB Devices Page](https://learn.microsoft.com/en-us/windows/wsl/connect-usb), usbipd-win requires:
* Running Windows 11 (Build 22000 or later). (Windows 10 support is possible, see note below).
* A machine with an x64/x86 processor is required. (Arm64 is currently not supported with usbipd-win).
* Linux distribution installed and set to WSL 2.
* Running Linux kernel 5.10.60.1 or later.

If you do not meet any of these specifications, please go directly to [Microsoft's Connect USB Devices Page](https://learn.microsoft.com/en-us/windows/wsl/connect-usb). Your Linux distribution and kernel should be adequate if you have recently installed WSL. 

## Step 2: Install ubipd-win

>Note: Restarting your PC may be required.

There are two methods to install usbipd-win: 

1. Directly installing the .msi installer from their [GitHub page](https://github.com/dorssel/usbipd-win).
2. By using winget. 

To install using winget, open a Windows **powershell**:
``` powershell
winget install --interactive --exact dorssel.usbipd-win
```

Proceed through the entire installation. Do not close the terminal or shut off your computer unless explicitelly asked to by the installer. 

## Step 3: Install Linux-side User-Space Tools and Hardware Identifiers

Open a WSL Ubuntu terminal. Type the following commands: 

``` bash
sudo apt install linux-tools-generic hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*-generic/usbip 20
```

These commands are only applicable if using Ubuntu.

After running `sudo apt upgrade` you may need to repeat the command 
``` bash
sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*-generic/usbip 20
```
if usbipd stops working for any reason. 

## Step 4: Reboot udev Rules

**Do not complete this step if you have not yet set udev permission for the crazyradios. This should have been completed as part of Step 4 of the [WSL Bitcraze Cfclient Setup](WSL_2_Bitcraze_Cfclient_Setup.md)**. 

First, reload the udev rules as we first did when setting them up in the first place:
``` bash
sudo udevadm control --reload-rules
sudo udevadm trigger
```

Then run the command
``` bash
udevadm control --reload
```
so that the udev service is restarted properly and that usbipd-win will work. If you get an error such as `Failed to send reload request: No such file or directory` then run the following command:
``` bash
sudo service udev restart
```
and then try to reload the udev control again: 
``` bash
udevadm control --reload
```

## Step 5: Connecting USB Devices to WSL 

By default, usb devices when plugged into your computer will not be available inside of WSL. To check this, first plug in your usb device. Then open a WSL terminal and enter the command 
``` bash
lsusb
```
and verify that the peripheral is not listed. 

To connect a device such as the crazyradio to your WSL distro you will need to open up a Windows **powershell** in **adminstrator mode**. 

To list all the available usb devices to attach and their respective bus-ids enter the following command into powershell:
``` powershell
usbipd wsl list
```
You may have certain internal devices such as bluetooth modules and webcams show up even if you do not have such devices hooked up to your usb ports. This is normal. 

Try typing this command both with and without the crazyradio device connected to your computer through bluetooth. Sometimes it may take a few seconds after plugging a new usb device in for it to be recognized. 

The `usbipd wsl list` command should have shown the bus-ids of all your usb devices. Find the bus-id of the crazyradio and use it to connect the device with the following command:
``` powershell
usbipd wsl attach --busid <BUS-ID>
```
where \<BUS-ID> is the bus-id outputted by the `usbipd wsl list` command. Common ids will be `3-1`, `3-2`, `4-1`, and `4-2`. It is important to note that the exact id may change each time you plug in your usb device. You should always check the bus-id before attempting to connect. Re-connecting with the previous commands will be necessary after each time you unplug the usb device. 

Verify that the device has been attached by again entering the command
```usbipd wsl list``` . The device should be listed as `ATTACHED`.

> If you are using a dongle or adapter to attach the crazyradio to your computer's port, the dongle or adapter may also show when listing available usb devices. We have found that it is generally not required to also attach the dongle or adapter as long as the actual device is connected. 

Now, open up a WSL terminal and type the following command to list all connected usb devices: 
``` bash
lsusb
```
you should find that your device is listed.

You are now ready for programs to access the crazyradio inside of WSL. 
