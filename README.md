# Surface_Go_Dualboot  
  
# Disclaimer:  
__**I AM NOT RESPONSIBLE if your Surface Go bricks, explodes, if you break into NSA, FBI, CIA, NASA databases, cause a nuclear meltdown or show any other "bad behavior" after using this guide.**__  
(Just to be sure people understand how terminal commands are powerful and not reading twice before executing is dangerous.)  

------  

Requirements:  
- 1x Type-C to USB A adapter  
- 1x 8Go+ USB Key  
OR  
- 1x 8Go+ Type-C USB Key  
- [Manjaro ISO](https://manjaro.org/download/)  
- [Rufus](https://rufus.ie/)  

A second usb key with the kernel binaries might be handy as you might not have Wi-Fi at first boot.  
I also used my phone as an USB tether device, so I could have internet with the initial wifi drivers ;)  


# I) Before Linux install :

1. Disable bitlocker / encryption (windows settings)  
2. Shrink your main Win 10 partition so you can get \~25G for the distro + 8G for swap  
3. Create your bootable usb with Rufusa  
Replace MBR with GPT partition table in Rufus and click start.  
Use the `dd` method when asked  
4. Shutdown your Surface go.  
5. Reboot while holding Volume Up + Power buttons  
6. Get to "Boot" and disable SecureBoot  
7. Put the USB boot entry before the Windows boot entry (just in case)  
8. Save & Exit  

__Now you should get to the grub menu on your usb key.__  
# II) Linux install :

1. Select your locals (timezone, language, keyboard, etc) & launch Manjaro  
1. Do the same steps in the installer ...  
2. Select `manually partition` when asked  
3. Select the 260 Mo partition (must be the first)  
 Set `/boot/efi` as the mount point.  
 Save and exit  
4. Select the partition you created in I) step 2 on Windows  
Click create  
5. Set the size to 8GB (or 4GB if you have the 4GB variant)  
 Change the `ext4` format to `linuxswap`.  
 Click save and exit.  
6. Create a partition with the remaining space.  
Set `/` as the mount point.  
Save and exit.  
7. Check and double check, f\* it **TRIPLE check** that you're not erasing anything you shouldn't.  
8. Let it install & reboot.  

# III) After Linux install :

1. Reboot on your Bootable USB Key  
2. Select "Detect EFI partitions"  
3. Select the Manjaro boot entry (ending with grubx64)    
5. Repeat steps III)

# IIII) Jakeday Kernel install :   
0. Open a terminal  
**Recommended**: `sudo pacman -Syu`  
1. `git clone https://github.com/dmhacker/arch-linux-surface.git ~/surface-kernel`  
2. `cd ~/surface-kernel`  
3. `sudo sh setup.sh`  
4. `sudo sh configure.sh`  
5. `cd build-{VERSION}` (replace `{VERSION}` with the actual version)  
6. `sudo chown -R user:user .` (replace `user` with your username)  
7. `MAKEFLAGS="-j$nproc" makepkg -sc`
6. Repeat steps III)  

# IIIII) Grub process :  

0. Open a terminal  
2. Type `efibootmgr`  
Take note of *Manjaro* and *EFI USB* entries' IDs (the 4-digit number on the left)  
For me, `Manjaro: 0002`, `Windows: 0000`, `EFI USB: 2001` and `EFI Network: 2002`  
3. **Recommended** : `efibootmgr -o {EFI USB ID},{ManjaroID},{WindowsID},{EFI Network ID}`  
Example : `efibootmgr -o 2001,0002,0000,2002`    
4. `grub-mkconfig -o /boot/grub/grub.cfg`  
5. **ENJOY!**  

------  

## Special thanks to :
- [Jakeday](https://github.com/jakeday) Linux-Surface kernel
- [DMHacker](https://github.com/dmhacker) Linux-Surface kernel's Arch fork
- [Arch Wiki contributors](https://wiki.archlinux.org/index.php/GRUB#UEFI_systems) Archlinux's _"Bible"_   
