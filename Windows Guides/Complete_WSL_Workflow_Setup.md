# Complete Guide to Setting Up Windows Based Workflow with WSL for Use with Crazyflies and Crazyswarm2
*Authored by [Eric Butcher](https://github.com/Eric-Butcher) (ericbutc@buffalo.edu) on September 16, 2023*
***

## Setting up WSL
First, set up WSL on your computer if you do not have it already. That guide can be found [here](WSL2_Ubuntu_Setup.md).

## Setting up the Bitcraze cfclient in WSL
Secondly, install Bitcraze's cfclient, a program used for basic interfacing with a crazyflie. It is used for updating firmware, changing radio addresses, and can allow you to fly a single crazyflie with an Xbox controller. That guide can be found [here](WSL_Bitcraze_Cfclient_Setup.md). This will not work without completion of the usbipd-win setup.

## Setting up USBIPD-WIN
Thirdly, install usbipd-win, a program that allows you to connect USB devices plugged into your Windows computer to your WSL distribution. This guide can be found [here](USBIPD_Setup.md).

> Note: udev permissions for connecting your crazyradio to your WSL distribution can get messed up after using apt update/upgrade. If you are having trouble with getting your crazyradio connected, try running the following commands: `sudo service udev restart` then `sudo udevadm control --reload-rules` and then `sudo udevadm trigger`. You may also need to run the command ```sudo update-alternatives --install /usr/local/bin/usbip usbip `ls /usr/lib/linux-tools/*/usbip | tail -n1` 20``` alongside the previous commands if it did not work. If you are still having trouble, please refer to the [bitcraze udev permissions documentation](https://www.bitcraze.io/documentation/repository/crazyflie-lib-python/master/installation/usb_permissions/) and the [usbipd WSL support documentation page](https://github.com/dorssel/usbipd-win/wiki/WSL-support).

## Installing ROS2 Humble
Fourthly, install ROS2 Humble. This is the current version of ROS2 that will be used with crazyswarm2. Official installation instructions are found [here](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html).

## Installing Crazyswarm2
Fifthly, install [Crazyswarm2](https://imrclab.github.io/crazyswarm2/). This is a program that allows for (relatively) easy control of individual and swarms of crazyflies. Official installation instructions are found [here](https://imrclab.github.io/crazyswarm2/installation.html).

