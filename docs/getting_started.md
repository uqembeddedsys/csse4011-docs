# CSIRO/CSSE4011 open repository setup guide

## Hardware
* JLink Debugger/Programmer e.g. JLink EDU Mini
* nrf52840 particle argon/xenon
* other boards may be supported, i.e nrf52840dk, boards that are supported are listed in ei_freertos/core_csiro/platform

## Software Installation:

The repo may be setup for Linux, OSX, or WSL, this guide will be using Linux. It is not recommended to use a virtual machine due to extended compilation times.

* VScode
    - https://code.visualstudio.com/Download  
    $ cd ~/Downloads  
    $ sudo dpkg -i code_1.26.0-1534177765_amd64.deb
* git  
    $ sudo apt install git
* Python 3
    - should be installed by default on ubuntu
    - make sure python3 
    - install Python headers and static libraries   
        $ sudo apt-get install python3-dev
    - install pip3  
        $ sudo apt install python3-pip  
* dialout group
    - add user to dialout group to acces /dev/tty devices
    $ sudo gpasswd --add ${USER} dialout
    - reset machine 
## gcc-arm-none-eabi 

For Debian try:
    $ sudo apt-get install gcc-arm-none-eabi

For Ubuntu try:
    $ sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa  
    $ sudo apt-get update  
    $ sudo apt-get install gcc-arm-embedded  

## Installing Segger JLink
    - download from https://www.segger.com/downloads/jlink/
    - J-Link Software and Documentation pack for Linux, DEB installer, 64-bit  
    $ sudo dpkg -i ~/Downloads/jlink_5.2.7_x86_64.deb


## Repository Setup

### Generate SSH Key
$ ssh-keygen -t rsa -C "example@example@uqconnect.edu.au"  
$ ssh-add  
then copy the key from id_rsa.pub to your github account

### Set up environment variables

add the following lines to ~/.bashrc 

- export PATH=<ei-freertos-path\>/tools:$PATH
- export PYTHONPATH=<ei-freertos-path\>/pyclasses:$PYTHONPATH

$ source ~/.bashrc

## Setup the Repository

    $ git clone https://github.com/uqembeddedsys/ei-freertos.git
    $ ./repo_setup.sh

## Compile and flash an app:

### compiling the app low_power:
    $ cd apps/low_power  
    $ make TARGET=argon debug -j

### flashing an app
connect your jlink EDU mini  
#### verifying JLink
    verify it is plugged in correctly and recognises the device
    $ JLinkExe
        > connect
        > S
        > <Enter>
    If a Target device selection comes up, select nrf52840_xxAA, by nordic semiconductors, Cortex-M4 on subsequent connects it will be the default device

### flashing the app
    $ make TARGET=argon flashdbg -j   
    blue LED should start blinking

### tips:
    $ make TARGET=argon help - generates help message  
    $ make TARGET=argon jlinksn - get serial numbers of devices connected - useful for multiple devices connected  
    $ make TARGET=argon JLINK=<serial number> - needed when multiple devices connected to computer or it will give you error, alternatively just take one out



