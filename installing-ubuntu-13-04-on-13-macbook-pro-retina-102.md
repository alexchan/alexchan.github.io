Title: Installing Ubuntu 13.04 on 13" Macbook Pro Retina (10,2)
Date: 2013-05-01 18:08
Author: alex
Category: Technology
Slug: installing-ubuntu-13-04-on-13-macbook-pro-retina-102

A couple of weeks ago, I purchased the 13" Macbook Pro Retina. I spent a
long time thinking about the purchase because I wanted something that
would work well with Ubuntu and had at least 1080p and 8GB RAM. Since
I've bought it, I gave OSX a try in the hopes that it will be usable for
development since I'm almost exclusively on Ubuntu for the last several
years. I think it's definitely usable but it's hard to change old habits
so I installed Ubuntu on the Macbook. The following is my process, my
mistakes and what I've learned.

<span style="line-height: 24px;">1. Partition</span>
====================================================

The first step is pretty simple.  You can resize the Macintosh partition
using the OSX Disk Utility.  I resized the partition to 88GB and left
the rest as free space.

2. Create USB Installer
=======================

You should follow the
directions <http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx> to
create a 64-bit Ubuntu USB startup drive.  
At the end of the instructions, you'll see an error message indicating
that OSX isn't able to read the drive.

[![Screen-Shot-2013-10-30-at-9.20.37-PM](/images/Screen-Shot-2013-10-30-at-9.20.37-PM-300x118.png)](/images/Screen-Shot-2013-10-30-at-9.20.37-PM.png)  
Don't worry, this is normal and it should still work as a USB installer
but you just won't be able to read/write to the drive itself.

(OPTIONAL but RECOMMENDED) Since you won't be able to read/write to this
drive, if you have another USB stick handy, you should upload the [wifi
driver](http://packages.ubuntu.com/raring/bcmwl-kernel-source "wifi driver")
to the 2nd USB stick.  After the Ubuntu installation, wifi will not work
and you will need to install the driver and its 3 dependencies to get it
working.

3. Install Boot Manager
=======================

I used rEFInd as the boot manager instead of rEFIt.  It seems to work
for me.  You can find it
here: <http://www.rodsbooks.com/refind/getting.html> and download the
binary zip file.  Once downloaded and unzipped, open a Terminal window
and install:

    cd ~/Downloads/refind-bin-0.6.7  
     ./install.sh 

4. Install Ubuntu
=================

Here comes the fun part.  Insert your Ubuntu USB installer and restart
your computer.  When the computer restarts, you should see your new boot
manager screen with an Apple and a triangle shaped icon for the Ubuntu
installer.  Right arrow over to the installer and hit Enter to select
it.  This will load the typical Ubuntu installer.  Select "Install
Ubuntu".  If this is the first time you are installing Ubuntu, it should
give you the option to install Ubuntu alongside Mac OSX.  If you are
reinstalling over a bad installation, it will give you the option to
erase Ubuntu only and reinstall.  **WARNING!** If you are attempting a
reinstall and select this option, the previous swap partition will not
be reused or erased.  A new one will be created meaning you will lose
about 8GBs of space for nothing.  Instead of having the installer create
the partitions for you, initial install or reinstall, I recommend
selecting the "Something else" option and create your own partitions.

If you select "Something else" you will see a screen with a list of
partitions.  If this is a reinstall, scroll to the bottom of the list
and delete the previous install's paritions.  At the very bottom of the
list is "Free space".  Select that and click on the "+" sign to create a
new partition.  You can create a root "/" using ext4 with a size of
about 25GB.  Next, create a swap partition with a size around 8 to 16GB.
 Finally, create an ext4 home "/home" partition with the rest of the
remaining space and continue with the installation.  After the
installation, it will ask you to reboot and you should do so.  If you
get a blank screen after the message, just hit the space bar and it will
continue.

5. Setting Up WIFI
==================

After you restart, you should see a Ubuntu logo, an Apple logo and the
Linux penguin.  You may also see the diamond shaped logo if your USB
stick is still plugged in.  If you didn't have a 2nd USB stick, you will
need to get the wifi drivers.  Select the Apple logo to boot into Mac
OS, format the USB installer so it can be read and copy the drivers from
the link above onto the USB stick.  Reboot.

When you have your wifi drivers, select the penguin, **NOT** the Ubuntu
logo.  If you select the Ubuntu logo, I think it will go to the grub
rescue screen and I had to hold down the power button to reboot again.
 After selecting the penguin, you should see the normal GRUB menu on the
left hand side.  Go ahead and select the first option, "Ubuntu" and
continue to log in and admire your beautiful Ubuntu installation.  You
can change the resolution by clicking on the Dash and selecting
"Displays" to make things easier to read.

Insert your USB stick with your wifi drivers if you haven't already done
so and open a Terminal. Change to the directory of the USB stick and
install each of the 3 dependencies first and then the wifi driver last.

    cd /media/[USERNAME]/[USB DRIVE]
    sudo dpkg -i  dkms*.deb
    sudo dpkg -i libc6*.deb
    sudo dpkg -i linux-lib*.deb
    sudo dpkg -i bcmwl*.deb

You should now have wifi access.  Woohoo!

6. Repair EFI Boot Manager
==========================

I needed to do this so that I could load Ubuntu's 64 bit EFI boot
manager.  In the terminal, enter the following commands:

    sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt-get update
    sudo apt-get install -y boot-repair && boot-repair

When the popup appears, click on "Advanced options" and go to the "GRUB
location" tab.  Check the box next to the option "Separate /boot/efi
partition" and click on "Apply".  You will be presented with a series of
commands to enter into the Terminal.  Follow the instructions.  I ran
into a dependency error on the last command I was presented with related
to efi-amd64.  Go ahead and continue forward anyway and you will get the
message that boot repair was successful.  Just for peace of mind, I ran
the boot-repair command again but was not presented with the commands to
enter and the final message indicated the repair was successful.  If
someone understands this section better than I, please let me know.
 **After rebooting, you should now select the Ubuntu icon and not the
penguin icon.**

7. Fix Sound Setting
====================

The last thing I had to fix was the sound setting.  Fortunately it was
extremely easy.  Edit the file: /etc/modprobe.d/alsa-base.conf and add
the following line to the bottom.

    options snd-hda-intel model=mbp101

8. Additional Notes
===================

At this point, you should be good to go. Everything else seems to be
working well and it's customizations from this point.  One important
note, **DO NOT INSTALL NVIDIA DRIVERS**.  This will screw up your
installation and you will not be able to boot back in.  Also, do not
install laptop-mode-tools as that will also prevent you from logging
back in.  In the event you do and you need to recover, boot from your
USB startup drive and select "Try Ubuntu".  Open a terminal and enter
the following commands while replacing "/dev/sda4" with your root
partition. The "Disks" utility can help you find out what that is.

    sudo mount /dev/sda4 /mnt
    sudo chroot /mnt

You will now be able to do things as if you're already logged in such
as:

    sudo apt-get purge laptop-mode-tools

### Boot loaders

I don't like seeing the Microsoft logo as an option.  I removed the
Microsoft directory from /boot/efi/EFI.  You can move it somewhere else
in case you want to put it back.

I like fast boot times.  If you log into the Mac OSX side and edit
/boot/refind/refind.conf, you can reduce the timeout to a few seconds so
that it will automatically load Ubuntu faster.  Also, on the GRUB side,
you can edit /etc/default/grub and reduce the timeout value there as
well.

I hope this helps someone save time instead of trying to figure it all
out.  I'd definitely like to hear from anyone if this helps them and if
there are any problems, please let me know!

9. References
=============

<http://randomtutor.blogspot.com/2013/02/installing-ubuntu-1304-on-retina.html>

<http://cberner.com/2013/03/01/installing-ubuntu-13-04-on-macbook-pro-retina/>

<http://commandline.org.uk/2013/03/08/lubuntu_on_macbook_pro_retina.html>

