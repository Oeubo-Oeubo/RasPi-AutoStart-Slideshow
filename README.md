# RasPi-AutoStart-Slideshow

## Description
This repo covers the process of setting up FEH ona raspberry pi to launch a slideshow of images on a flash drive, or in this case a floppy disk reader. We will be using a Raspberry Pi 3 B+ and a Tendak floppy disk reader with Verbatim floppy disks.

## Starting Up From Scratch
Download Raspberry Pi Imager on a desktop or laptop and flash the most recent recommend OS onto a MicroSD card. I am using Debian Bullseye which is 0.9 GB. Use any MicroSD card that can fit the OS file size multiplied by 3. An 8GB will work for this project. 

When you first turn on the Raspberry Pi with the MicroSD containing the OS is inserted, follow the instructions normally and select all the recommended settings. Just make sure the "Boot / Auto Login" setting in "Systems Options" has "Desktop Autologin: selected. You can select this later by opening terminal and typing "sudo raspi-config".

