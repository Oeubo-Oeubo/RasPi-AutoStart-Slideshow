# RasPi-AutoStart-Slideshow

## Description
This repo covers the process of setting up FEH on a Raspberry Pi to launch a slideshow of images from a plugged-in flash drive, or in this case a floppy disk reader. We will be using a Raspberry Pi 3 B+ and a Tendak floppy disk reader with Verbatim floppy disks .

## Starting Up From Scratch
Download [Raspberry Pi Imager](https://www.raspberrypi.com/software/) on a desktop or laptop and flash the most recent recommend OS onto a MicroSD card. I am using Debian Bullseye which is 0.9 GB. Use any MicroSD card that can fit the OS file size multiplied by 3. An 8GB will work for this project. 

When you first turn on the Raspberry Pi with the MicroSD containing the OS is inserted, follow the instructions normally and select all the recommended settings. Just make sure the "Boot / Auto Login" setting in "Systems Options" has "Desktop Autologin: selected. You can select this later by opening terminal and typing ```sudo raspi-config```.

## Creating Our Slideshow Files

### Step 1: Installing dependencies
We need to install FEH for this project. It is a lightweight and powerful image viewer that can be called using command line code. 

In a new terminal window, insert these commands.
```sudo apt-get install feh```

### Step 2: Creating a .desktop file.
We will create our own Desktop Entry by programming a .desktop file that loads when the Raspberry Pi boots up. This route is best for GUI-based Raspberry Pi programs like this one.

Same or new terminal window, insert these commands.
```
cd ~
sudo nano /etc/xdg/autostart/[insert new file name].desktop
```
With the file opened inside nano, insert this code.
```
[Desktop Entry]
Name=Slideshow
Exec=lxterminal -e sh autostart-feh.sh
Type=Application
```
Name can be anything you want it to be.

In Exec, I HIGHLY RECOMMEND using lxterminal to execute commands. It opens a new terminal window that gives you the ability to exit out of any programs you start. I've forgotten to do this before and had to flush the OS because I could never escape the program that was running. We will be creating a .sh file, known as a shell file, in the next step. Here we want to execute the shell file that we will create "anykindofname.sh".

Type must be Application.

Ctrl + S to save and then Ctrl + X to exit.

### Step 3: Mounting the flash drive manually.
Now we are going to mount our flash drive. There is a chance it doesn't already do this automatically and we will be doing this as a precaution. First we need to plug in our flash drive and open file manager. If it is already mounted, type this in the terminal window.
```
df -h
```
You will see your flash drive under Filesystem = /dev/sda and Mounted on = /media/[user name]. The paths might be slightly different based on the type of drive you have.

### Step 4: Creating a shell file.
Same or new terminal window, insert these commands.
```
cd ~
sudo nano autostart-feh.sh
```
I am naming my file autostart-feh.sh. Again, you can name it anything just make sure it is has the .sh extension.
We are going to create a while loop that will keep looking for our drive incase FEH every crashes or the drive is not plugged in.
```
while:
do
[our code]
sleep 2
done
```
We will be placing all of our commands between do and sleep 2. Sleep delays the loop by 2 seconds so we don't overwhelm our terminal messages for every failed load.

In our code area, we want to start by mounting our drive. This process will keep looking for a new drive and mount it when the previous one is not detectable.
```
sudo mount /dev/sda /media/[username]
```

After the drive is mounted, we can change our directories to the drive's location.
```
cd ~
cd /media/[username]
```

Now FEH will come into place. I want a looping slideshow of all .jpg files inside of a flash drive, or my floppy disk.
```
sudo feh --recursive --fullscreen --auto-zoom --quiet --hide-pointer --slideshow 3 --reload 3 --on-last-slide resume
```
These settings, in order, make my images fullscreen, fit to screen, hide the mouse icon, changes between images every 3 seconds, double checks any changes to the flash drive every 3 seconds, and then loops once reaching the last file. You can find a deeper explanation of [all FEH settings here](https://man.finalrewind.org/1/feh/).

I am going to add some echo commands that will print what is happening under the hood in terminal. Your code should look similar to this.
```
echo "Starting slideshow program process."
while:
do
echo "Mounting drive..."
sudo mount /dev/sda /media/[username]
echo "Opening files..."
cd ~
cd /media/admin
sudo feh --recursive --fullscreen --auto-zoom --quiet --hide-pointer --slideshow 3 --reload 3 --on-last-slide resume
echo "!!! Files not detectable. Starting to remount drive. !!!"
echo "!!! If no flash drive is detected, please insert now. !!!"
sleep 2
done
```
