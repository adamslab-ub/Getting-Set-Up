# Complete Guide to Setting Up Windows Based Workflow with WSL for Use with Crazyflies and Crazyswarm2
*Authored by [Eric Butcher](https://github.com/Eric-Butcher) (ericbutc@buffalo.edu) on September 16, 2023*
***

## Setting up WSL
First, set up WSL on your computer if you do not have it already. That guide can be found [here](WSL2_Ubuntu_Setup.md).

## Setting up the Bitcraze cfclient in WSL
Secondly, install Bitcraze's cfclient, a program used for basic interfacing with a crazyflie. It is used for updating firmware, changing radio addresses, and can allow you to fly a single crazyflie with an Xbox controller. That guide can be found [here](WSL_2_Bitcraze_Cfclient_Setup.md). This will not work without completion of the usbipd-win setup.

## Setting up USBIPD-WIN
Thirdly, install usbipd-win, a program that allows you to connect USB devices plugged into your Windows computer to your WSL distribution. This guide can be found [here](WSL_2_USBIPD-WIN_Setup.md).

## Installing ROS2 Humble
Fourthly, install ROS2 Humble. This is the current version of ROS2 that will be used with crazyswarm2. Official installation instructions are found [here](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html).

## Installing Crazyswarm2
Fifthly, install [Crazyswarm2](https://imrclab.github.io/crazyswarm2/). This is a program that allows for (relatively) easy control of individual and swarms of crazyflies. Official installation instructions are found [here](https://imrclab.github.io/crazyswarm2/installation.html).