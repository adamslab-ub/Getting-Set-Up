# Installing the Bitcraze cfclient on a WSL Ubuntu Environment Tutorial
*Authored by [Eric Butcher](https://github.com/Eric-Butcher) (ericbutc@buffalo.edu) on September 16, 2023*
***

## Introduction

This will be a step-by-step tutorial for installing the cfclient on an Ubuntu WSL environment. It should be noted that the connect usb-peripherals to the Linux machine, we will have to use a tool called usbip. This means that while this tutorial will allow you to run and launch cfclient on your WSL enviornement, you will not be able to connect to crazyflies through the cfclient until you get that program set up. 

Using cfclient through WSL has the significant advantage of allowing users to do crazyflie/swarm work on their own Windows computer without the need for a dedicated Ubuntu laptop. Using WSL allows performance with near-native speed and latency. While crazyswarm2 and cfclient can technically be used through a VM, this can cause issues with some of the programs working poorly, being buggy, or having too high latency to be practically usable for anything but the most simple of administrative tasks such as updating firmware and addresses. In our experience, using cfclient through a VM caused numerous issues with the program's GUI not rendering properly and being incredibly laggy regardless. This made it difficult to do even the most simplest of tasks, and impossible to do others. Depending on the VM, there was also an apparent latency of communication of anywhere from 4-20 seconds when connected to the crazyflie over antenna. The latency with WSL is unnoticeable. 

## Step 1: Ensure installation of Python3 
Python 3.10.x should already be installed on Ubuntu 22.04. To check if it is installed, run the following command in the terminal:
``` bash
python3 --version
```
If python3 is not installed, please search online on how to install it onto your machine without interfering with other python installations you may have on your machine. 


## Step 2: Update and upgrade system

Ensure that your apt packages are up-to-date:
``` bash
sudo apt update && sudo apt upgrade
```

## Step 3: Ensure installation of needed dependencies

Ensure that you have git, pip3, and libxcb-xinerma0 installed on your system: 
``` bash
sudo apt install git python3-pip libxcb-xinerama0
```

Upgrade pip3:
``` bash
pip3 install --upgrade pip
```

## Step 4: Set udev Permissions

This step will allow you to connect with bitcraze devices such as the crazyradio through usb without being root. Go to [Bitcraze's article on USB permissions](https://www.bitcraze.io/documentation/repository/crazyflie-lib-python/master/installation/usb_permissions/) and follow all the steps there. 

Since we are using WSL, the usb be devices will not work by default. Getting usb devices working properly in the WSL environment will be covered in the [usbip setup guide](). Proceed to the next step and make sure to go to that guide after completing this tutorial. 


## Step 5: Install cfclient from pip3

While cfclient can also be installed from source, installing it through pip3 in preferred. If you use virtual python environments, you should create an environment for working with crazyflies before installing cfclient. 
``` bash
pip3 install cfclient
```
**Do not clear your terminal.**

You should now be able to launch cfclient by typing the following in your terminal: 
``` bash
cfclient
```
Verify that cfclient launches properly. If cfclient launches properly, you are done. 

If cfclient does not launch for any reason, first store the output of the cfclient installation into a safe place and then close and reopen your terminal. Try typing the command in again. If this still does not work, open a Windows **powershell** and type:

``` powershell
wsl --shutdown
```
and then close any currently open WSL windows/terminals. This will effectively restart WSL. Then open a new Ubuntu terminal and try typing cfclient again. 


We have found a bug that we experience with some cfclient installations. If you are still having problems running cfclient, it may be because of the default PyQt5 installation on your machine. If you are getting a python error when running cfclient mentioning and GUI or PyQt parts, this is likely the issue. To fix this bug, run the following command in the terminal:

``` bash
sudo apt-get remove python3-pyqt5
pip uninstall pyqt5
sudo apt install python3-pip
pip install pyqt5 
```

Then try using cfclient again. 

You should now be able to use cfclient. 