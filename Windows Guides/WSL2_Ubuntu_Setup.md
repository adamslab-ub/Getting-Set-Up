# Setting up an Ubuntu environement in WSL2 on Windows 11
*Authored by [Eric Butcher](https://github.com/Eric-Butcher) (ericbutc@buffalo.edu) on Sep 1, 2023*
***

# Introduction

>Note: At the time of creating this documentation, the latest version of WSL is WSL2 and the default Linux distribution is Ubuntu 22.04LTS when setting up WSL from scratch with the regular 'Ubuntu' identifier. Methods for setting up new versions of WSL may change, as they have changed significantly from WSL1 to WSL2. Additionally, this tutorial assumes that that user wishes to install Ubuntu 22.04LTS as their distribution, and that this is the version of Ubuntu that is most compatible with the version of ROS2 that will later be installed. Please check before using this tutorial that these assumptions are still true, and update this documentation if necessary to accomodate future changes. 

>Note: This tutorial assumes that the user is installing WSL for the first time on their machine with no prior experience. Portions of this tutorial, especially in the begining, may change if the user has already installed WSL before, whether or not they currently have WSL installed. If this is the case please refer to the [official Microsoft WSL documentation](https://learn.microsoft.com/en-us/windows/wsl/) to assist yourself. If you currently have a satisfactory WSL2 installation on your machine, please continue to [insert file link here]() to install usbid so that you can connect the bitcraze crazyradio to WSL.

This tutorial was mostly taken from [Microsoft's 'How to install Linux on Windows with WSL' documentation page](https://learn.microsoft.com/en-us/windows/wsl/install) along with [Microsoft's 'Manual installation steps for older versions of WSL'](https://learn.microsoft.com/en-us/windows/wsl/install-manual). The goal of this tutorial is to alleviate common problems with installing WSL the simplest way without going into the complexity of a manual install. However, if you encounter problems, please refer to the referenced documentation. These instructions are built assuming the user is on Windows 11 with a build of 19041 or higher or Windows 10 with a build of 2004 or higher. For using WSL on a build of Windows that is older, please go straight to [Microsoft's 'Manual installation steps for older versions of WSL'](https://learn.microsoft.com/en-us/windows/wsl/install-manual). 

The Windows Subsystem for Linux (WSL) is a way for running Linux distriutions directly on your computer. Taken from [Microsoft's 'Frequently Asked Questions about Windows Subsystem for Linux'](https://learn.microsoft.com/en-us/windows/wsl/faq): 
>The Windows Subsystem for Linux (WSL) is a feature of the Windows operating system that enables you to run a Linux file system, along with Linux command-line tools and GUI apps, directly on Windows, alongside your traditional Windows desktop and apps.

We will use WSL because it is generally faster than using a virtual machine and provides a better experience when having to use Windows as your primary desktop environment. However, WSL is not as user-friendly in all scenarios as virtual machines. While WSL can run graphical programs, they must be launched from the command line, as WSL provides only a terminal interface to interact with your Linux distribution. This is perfectlly suitable for our purposes, but new users should be aware of this, especially if they are inexperienced with using Command Line Interfaces (CLIs). If you are inexperienced interacting with a computer through a CLI, please learn or brush up on using a Linux command line. Canonical (the maker of Ubuntu) has a great tutorial on [The Linux command line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview), although this tutorial assumes that the user has access to an Ubuntu desktop enironment. However, you should still be able to follow along just fine. The speed-up from using WSL over virtual machines for interactig with crazyflies is crucial. When tested, an Ubuntu VirtualBox VM has about a latency of 20 seconds when using cfclient. An Ubuntu VMWare Workstation VM has a latency of about 4 seconds when using cfclient. An Ubuntu WSL setup had ZERO NOTICEABLE LATENCY when using cfclient. Longer latenices may not be a problem for very basic work with crazyflies such as updating identifiers, firmware, or some basic scripts. For these purposes a VM may still be used, although we have not found a task that WSL could not do aswell, so there is really no point. 

>Note: usbip, a tool that at the time of writing this tutorial is necessary for connecting usb devices to WSL so that software can make use of devices such as the crazyradio, is only available on Windows 11 with builds of 22000 or higher. This WSL installation will likelly be completelly useless if you cannot connect crazyflies to any ROS or crazyflie software through the crazyradio. Additionally, usbip is not perfect and not all devices connect properly. In testing we found that the crazyradio dongle can cannot through usbid just fine, however, usbip cannot connect a manual cord connection from the crazyflie to the computer at this time. 

# Step 1: Update Windows to Latest Version

Especially if you have not updated Windows in awhile, please first install any updates from Windows Update.

# Step 2: Enable required Windows features

>Note: During this installation process the user must restart their computer. We also recommend restarting your computer after more steps than might be strcitly necessary because in our experience it has made the WSL installation process more reliable and less prone to error. Make sure that you have saved all work and closed any extraneous programs before starting this tutorial. 

While the Microsoft documentation claims that the modern installation process of WSL should enable the required Windows features by default, we have encountered problems, and belive it is better to manually ensure that these features are enabled before completing the WSL installation. 

Go to your Search in your taskbar and search for a control panel application called "Turn Windows features on or off." Open it. Make sure that the folders for "Virtual Machine Platform" and "Windows Subsystem for Linux" are enabled. If not, click the box next to their folder icons so that they are turned blue and are checked. Then click "Okay." 

Restart your computer. 

# Step 3: Ensure Installation of modern Windows Terminal application

There are many different programs available for working with a Windows machine through a CLI. In the past Windows users used cmd, and later, powershell. In 2019, Microsoft started to release a applicaiton called terminal. This allows you to open up multiple different CLI's in an application with tabs, similar to how you can have multiple websites opened in a web browser each in their own tab. While most modern Windows machines should come with this installed be default, some users may not have it by default. Make sure that this application is installed. If not, go to the Microsoft store and [install it](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701). 

# Step 4: Install WSL2 with Ubuntu

Open a Windows cmd or powershell (powershell is preferred) in **adminstartor mode**. This can be done by right clicking the application and choosing the "Run as administrator" option or by opening the Windows terminal application and ctrl+clicking on powershell under the drop-down menu next to the new tab + button. 

WSL supports multiple different Linux distributions (distros). At the time of writing this article, the default distribution for WSL is Ubuntu 22.04LTS. This is the version of Ubuntu that is currently supported by crazyswarm2/ROS2-Humble, which is the stack we are currently using. The most up-to-date version of the crazyswarm2 library is likely to change in the future, so please consult [their website](https://imrclab.github.io/crazyswarm2/installation.html) and other members in the lab for which distribution you should install.

If the default WSL distribution is suitable for your needs (as in our case), type the following command into the administrator powershell: 
```
wsl --install
```

If you require a different distribution, or the previous installation attempt failed, you can check which distributions are available to install from the command line with this command:
```
wsl --list --online
```

Which should output something like this:
```
PS C:\Users\myuser> wsl --list --online
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME                                   FRIENDLY NAME
Ubuntu                                 Ubuntu
Debian                                 Debian GNU/Linux
kali-linux                             Kali Linux Rolling
Ubuntu-18.04                           Ubuntu 18.04 LTS
Ubuntu-20.04                           Ubuntu 20.04 LTS
Ubuntu-22.04                           Ubuntu 22.04 LTS
OracleLinux_7_9                        Oracle Linux 7.9
OracleLinux_8_7                        Oracle Linux 8.7
OracleLinux_9_1                        Oracle Linux 9.1
openSUSE-Leap-15.5                     openSUSE Leap 15.5
SUSE-Linux-Enterprise-Server-15-SP4    SUSE Linux Enterprise Server 15 SP4
SUSE-Linux-Enterprise-15-SP5           SUSE Linux Enterprise 15 SP5
openSUSE-Tumbleweed                    openSUSE Tumbleweed
```

You can then type the name of the distribution you want to install into this command, replacing the entirety of \<Distribution Name> with the name of the distribution you want to install from above:
```
wsl --install -d <Distribution Name>
```

Leave the installation alone until it finishes. **Do not close the terminal**. When you receive a prompt to enter your UNIX username, proceed to the next step. If you have trouble with the installation, please refer to [Microsoft's 'Troubleshooting Windows Subsystem for Linux' offical documentation](https://learn.microsoft.com/en-us/windows/wsl/troubleshooting#installation-issues).

# Step 5: Setting up UNIX

You should be prompted for a UNIX username. Your UNIX username is the name of the specific user of a Linux computer, similair to profiles on Windows. Your selected UNIX username will appear every time you use the Linux terminal in the following format:
```
username@MachineName:~$
```
So chose something short that you like!

Next it will prompt you to enter a password. When typing your password it will not show the characters being typed or any dots like you may be used to when typing in a password to a website or other operating systems. This is a common behavior with command lines in a UNIX environment. This password will be used any time you need to type a 'sudo' command (the Linux equivalent of doing something with adminstrator privelages in Windows). You will be doing this __very__ often. Make sure you remeber your password. 

After this step is done you should be prompted with a terminal that will look similair to the following: 
```
username@MachineName:~$
```
Type exit and press enter as shown to return to the Windows powershell:
```
username@MachineName:~$ exit
```

# Step 6: Update and Upgrade System

Whenever you first intall a new Debian-based Linux operating system (such as Ubuntu) it is good practice to update and upgrade your system packages before you do anything else. 

Re-open the terminal application and use the drop-down menu to open a new terminal tab of the Linux distribution you just installed. You may also want to go into settings in the future so that this terminal opens by default when you open the terminal application. 

>apt stands for "Advanced packaging tool" and is similair to the app stores you may be familiar with on iOS and Android, except apt contains packages (programs) for Debian-based Linux distributions and is primarily used through the command line instead of through a GUI. 

First we will update our apt package list. This will fetch all of the most recent information about the packages we can install on Ubuntu:
```
sudo apt update
```
Make sure to answer 'y' if asked. 

Next we well upgrade of the current programs installed on Ubuntu since we now know of the most recent changes since updating our package lists:
```
sudo apt upgrade
```
Again, make sure answer 'y' if prompted. 

These two steps together may take some time. 

>apt is a tool that you will be using often with Ubuntu. However many tutorials may instead use the older apt-get instead. These two commands/programs are largely interchangeable. However, it is recommended to use apt instead whenever possible. 

It is recommended that you always update and upgrade your packages before you install new software. In the future you may wish to do this with the combined:
```
sudo apt update && sudo apt upgrade
```
