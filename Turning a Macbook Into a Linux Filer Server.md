# Turning an old Macbook into a Debian Server
I have an old Macbook sitting around collecting dust. As a fun homelab project I decided to try and install Debian Linux on it and using as a file server, or whatever else strikes my fancy.

I do apologize for some of the low quality "screenshots" that are to follow, as obviously attaching a capture card to a laptop to get installation footage is kind of a pain in the butt, not to mention the fact that I don't have a capture card.

The Macbook is an intel Mac from around 2019 running Mojave 10.14.6. Results for M1 Macs may vary.

If you plan on dual booting this setup, before installation you will want to create a [new partition](https://support.apple.com/guide/disk-utility/partition-a-physical-disk-dskutl14027/mac) for your Linux install using Disk Utility in Mac.

**Mac Info IMAGE**

First thing's first is to prepare my installation media. I have a thumbstick with [Ventoy](https://github.com/ventoy/Ventoy) installed on it. If you're not familiar with [Ventoy](https://github.com/ventoy/Ventoy), it's a great tool that allows you to have multiple ISO's on a single flashdrive. It's extremely easy to use, all you need to do is drag and drop your ISO into your Ventoy flash drive.

![Ventoy2](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/37719c5d-dcaa-4c5d-a498-9f91ba855f17)

I drop an ISO of Debian 12 Bookworm onto the flash drive. Debian has recently made their installation process much easier. You no longer need to go hunting for the ISO version that will allow installation of non-free drivers. I also personally prefer to install Debian stable, and then switch to the Testing or Sid (Unstable) branch after install, as I've had times where the weekly release has bugs during install.

All you need to do is go to [debian.org/download](https://www.debian.org/download) and download the link at the top of the page. After that just drag and drop the iso to your Ventoy drive.

![Debian-Installer](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/4e17e765-9872-4eb4-81f1-31344591f07b)

## Debian 12 Install
Now comes the fun part. Shut down the Mac and turn it back on while holding the *option* key. You will see the boot manager with a list of bootable devices. Select the one labeled "EFI Boot"

![20230812_210126](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/de7f884a-e654-42df-b5d4-0318b1f4d529)

This will take you to your Ventoy drive. 

![20230812_210206](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/178778e1-eb28-4877-b2fd-c9e9bde95878)

From here select the Debian ISO, and then boot into Grub2 Mode. From here you will get the Debian 12 install screen. I usually choose the "Install" option rather than the "Graphical Install" option as it's the same installer and every now and again the Graphical Installer can cause bugs due to video driver issues. In this case, the graphical installer will display all text very small, do the "retina" display. The track pad will also not work, so you would need a USB mouse.

![20230812_210537](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/cae637dc-0e70-4f26-a609-2857fd31dcf7)

Proceed with the install as normal

The first speed bump you might hit is a message saying that "Some of your hardware needs non-free firmware files to operate." It will complain about `brcmfmac43602-pcie.Apple` and ask you to insert media containing these files. This file is related to the Wifi adapter, but is actually not needed. Wifi will work fine and you can just select "No" on this screen.

![20230812_211558](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/dd39f3af-1e6b-40bb-a31e-40f21f92a649)

Continue with the installation, specifying and connecting to your wifi network. When asked for a domain name, leave it blank, you probably don't have one.

#### Don't Set a Root Password
When prompted for the root password, it is generally a good security recommendation to leave it blank, this will disable the root password. You can still elevate to root if necessary with `sudo su`, but disabling the root password eliminates one vector of privilege escalation if an attacker were to get on your system.

For the user password, if you plan on having this system accessible to you from anywhere in the world, it's a very good idea to actually use a strong password, rather than the simple one's most people use for their home systems. Later on, it is also a good idea to set SSH to only accept private keys, to eliminate password brute forcing altogether.

#### Partitioning
At the the "Partition Disks" screen, I select "Manual". I then select the APPLE SSD and hit enter. I get a message telling me that this will wipe out this entire partition,  I hit yes. Now back on the partition screen, it shows 500GB of free space on the Apple SSD. I then select the free space, tell it to automatically partition the free space, and select "All files in one partition". On the main screen it now shows a partition table for Linux. From here I select "Finish partitioning." Do be sure that there is nothing on the Mac install you need, as it will really and truly be very hard to recover.

If you are dual booting, in this case you would select the free space you allocated in your partition, tell it to automatically partition the free space, and select "all files in one partition".

![20230812_212224](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/18a7f0ad-4e93-4505-adaf-d46b1209d9ab)
![20230812_212536](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/207e96b5-4837-46aa-a805-867c08d307a2)
![20230812_212551](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/7d2280a2-c48a-4624-9588-82560bf3afb0)
![20230812_212558](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/ef2591c3-3bed-4de7-9026-28b3653ca88c)
![20230812_212606](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/359ae914-52b2-4918-8944-90f0cab9974b)
![20230812_212614](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/1d98ae94-ef2b-4fc0-b182-12601b84630b)
![20230812_212625](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/1d3dc881-530f-48c8-9d98-dbe756665fca)

On the mirror screen, if you don't know your fastest mirror, just select the default "deb.debian.org". When asked about an HTTP proxy, just leave it blank, as you likely will know if you have one.

On the Software Selection screen, I deselect "Gnome" and "Debian Desktop Enviroment", as I don't plan on using a DE, and select SSH Server.

![20230812_213407](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/d5aebdae-50d5-413f-84dd-b6d292edd8da)

After this, proceed with the installation as normal, and soon enough you will boot into your new Debian-Macintosh abomination machine! The clashing of two worlds, the Closed Garden meets Free as in Freedom! How wonderful it is.

Now from here you can just SSH into your new system, stick the laptop in a drawer and call it good. However, I do want to do some tweaks just in case I ever need to use the Mac itself. 

## TTY Font Size
The first thing you'll likely notice is that the text is quite small, this is due to the "retina" display of the Mac. The second thing you'll notice is that the keyboard backlight doesn't work, and the third is the screen backlight buttons don't work.

For the font size, the simplest solution I have found is to run `sudo dpkg-reconfigure console-setup`, which will take you through a setup wizard for you TTY. I selected VGA as the font, and the max size available.

**Before:**

![20230812_221059](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/1ba523bf-9f0e-440b-b5f2-444ee49e28bd)

**After:**

![20230812_221033](https://github.com/ClaytonReardon/Turn-a-Macbook-Into-a-Linux-File-Server/assets/112681383/792996cd-e9ac-4528-8944-5e552cdeb99a)

## Keyboard and Screen Backlights
For the keyboard and screen backlight there's a couple ways to do this. The backlight values can be manually changed in the files located at `/sys/class/leds/`. For example, you can run as root:
```bash
echo 255 > /sys/class/leds/smc::kbd_backlight
```
to set max keyboard brightness and:
```bash
echo 1388 > /sys/class/backlight/intel_backlight/actual_brightness
```
to set max screen brightness. You could from here devise a script to do this for you. A fun little project I may try one day.

There is an easier way however with the package `brightnessctl` which can be installed with `sudo apt install brightnessctl`. Once installed you can run `brightnessctl --list` to see the available devices. You can then change the brightness of the keyboard and screen with:
```bash
sudo brightnessctl --device='smc::kbd_backlight' set 255 # Keyboard
sudo brightnessctl --device='intel_backlight' set 1388 # Screen

```
In reality, this is probably all the functionality I'll ever need. I'll add an alias in my .zshrc for easy manipulation of these values. 
```bash
alias kbdlvl='sudo brightnessctl --device="smc::kbd_backlight" set '
alias scrlvlf='sudo brightnessctl --device="intel_backlight" set '
```
If you want, you can set up keyboard shortcuts to get the native functionality of the brightness keys, but that's a rabbit hole I don't feel like going down right now.

## Disable Sleep on Lid Close
The last thing I want to do is disable sleep when the laptop is closed. I want the screen to still go blank to save power, but for the Mac to still be on and usable. That way I can stick it on a shelf and just interact with it over SSH.

The steps to do this are actually fairly simple. If you run 
```bash
cat /proc/acpi/wakeup 
```
you should see a line similar to 
```bash
LID0	  S4	*enabled   platform:PNP0C0D:00
``` 
Take the value `PNP0C0D:00`, or whatever it is for you, and then run 
```bash
echo 'PNP0C0D:00' | sudo tee /sys/bus/acpi/drivers/button/unbind
```
After that, you should be able to close the lid without loosing power. However, after reboot, this functionality will be reset. To prevent that, I need to create a systemd service.

First, create an `/etc/rc.local` file that looks something like this:
```bash
#!/bin/bash

# Disable hibernate on Lid Close
echo 'PNP0C0D:00' | sudo tee /sys/bus/acpi/drivers/button/unbind

exit 0
```
Make sure you have that `exit 0`, it won't work without it. Set the permissions to make this executable. I ran `sudo chmod 700 /etc/rc.local` as that will make the file read/write/execute only by root. For security reasons, make sure other users cannot execute this file, absolutely cannot write to this file and for good measure, can't read it either.

After this I need to create a systemd service to execute this file. Create a file in `/etc/systemd/system/rc.local.service` with these contents:
```bash
[Unit]
Description=Execute /etc/rc.local
ConditionPathExists=/etc/rc.local

[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
StandardOutput=tty
RemainAfterExit=yes
SysVStartPriority=99

[Install]
WantedBy=multi-user.target
```
Then just enable that service with `sudo systemctl enable rc.local`. After this, reboot the system and make sure it worked. You should be able to shut the lid without loosing power. from here you can just run:
```bash
sudo brightnessctl --device='intel_backlight' set 0
sudo brightnessctl --device='smc::kbd_backlight' set 0
```
To kill the backlights, and bada bing bada boom! You can now stick this thing on a shelf and just SSH into it when neccesary! Very Cool.

This laptop has really had an interesting life. It was given to me a by a friend. It was her old work laptop, that her work had never asked to get back. She didn't need it, and she couldn't really use it anyways, as she needed the Administrator password to reset it. She gave it me to mess around with. The IT team at her old work was very bad, as all I had to do was run `sudo -l` to find out that I could run all commands as root, without even needing a password! So insecure! It's a miracle they never got hacked. (Although come to think of it, maybe they did. Maybe that got ransomwared and went out of business, and that's why they never asked for the laptop back.) From there I just searched how to reset a Mac password from the command line, reset the Admin password, and the laptop was mine! Amazing.



