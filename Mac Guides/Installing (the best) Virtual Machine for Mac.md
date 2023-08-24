# Installing (the best) Virtual Machine for Mac
*Authored by [Puru Soni](https://github.com/puru-soni-04) (purusoni@buffalo.edu) on Aug 24, 2023*
***

# Introduction 
We will be using a Virtual Machine to run Windows on our Mac. There are many Virtual Machines available for Mac, but we will be using [UTM](https://mac.getutm.app/). UTM is a free and open-source virtual machine that lets you run any operating system on your Mac. It is a very powerful virtual machine and is very easy to use.

From [UTM's Website](https://mac.getutm.app/): 
> UTM employs Apple's Hypervisor virtualization framework to run ARM64 operating systems on Apple Silicon at near native speeds. On Intel Macs, x86/x64 operating system can be virtualized. In addition, lower performance emulation is available to run x86/x64 on Apple Silicon as well as ARM64 on Intel. For developers and enthusiasts, there are dozens of other emulated processors as well including: ARM32, MIPS, PPC, and RISC-V. Your Mac can now truly run anything.

# Installing UTM
1. Go to [UTM's Website](https://mac.getutm.app/) and download the latest version of UTM.
2. Open the downloaded file and follow the instructions to install UTM.

# Installing Ubuntu (22.04)
We would need to install the Version of Ubuntu that matches the system architecture of our Mac.

It should either be `amd64 (also known as X86-64)` or `arm64`. 

`amd64` is for Intel Macs and `arm64` is for Apple Silicon Macs.

If you are not sure search online to find out which one is the correct one for your Mac.


## For Apple Silicon Macs (arm64)
1. Go to https://cdimage.ubuntu.com/jammy/daily-live/current/ (must use this website, the ARM .iso is not public) and download the latest version of Ubuntu (22.04) for `arm64`. Direct link: https://cdimage.ubuntu.com/jammy/daily-live/current/jammy-desktop-arm64.iso

2. Open UTM and click on the `+` icon on the top right corner of the screen.

## For Intel Macs (amd64)
Although not tested yet, you can try to install the `arm64` version of Ubuntu on your Intel Mac. The steps after download should be the same as the ones for Apple Silicon Macs.

1. Go to [Ubuntu's website](https://releases.ubuntu.com/jammy/) for version 22.04 and download the AMD64 version of Ubuntu. Direct link: https://releases.ubuntu.com/jammy/ubuntu-22.04.3-desktop-amd64.iso


