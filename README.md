# Surface_Go_Dualboot  

# Disclaimer:  
**I AM NOT RESPONSIBLE if your Surface Go bricks, explodes, if you break into NSA, FBI, CIA, NASA databases, cause a nuclear meltdown or show any other "bad behavior" after using this guide.**  
(Just to be sure people understand how terminal commands are powerful and not reading twice before executing is dangerous.)  

------
Requirements:  
- 1x Type-C to USB A adapter  
- 1x 8Go+ USB Key  
OR  
- 1x 8Go+ Type-C USB Key

A second usb key with the kernel binaries might be handy as you might not have Wi-Fi at first boot.

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
3. Select the 260 Mo partition (must be the first) and click Modify  
Set `/boot/efi` as the mount point.  
Save and exit  
4. Select the fresh and free partition you created in I) step 2 on Windows and click create  
5. Create an 8G (or 4G if you have the 4G variant) and change the \`ext4\` format to `linuxswap`. Click save and exit.  
6. Create a partition with the rest.  
Set `/` as the mount point.  
Save and exit.  
7. Check and double check, f\* it **TRIPLE check** that you're not erasing anything you shouldn't.  
8. Let it install.  

# III) After Linux install :

1. Reboot on your Bootable USB Key  
2. Select "Detect EFI partitions"  
3. Select the Manjaro boot entry (ending with grubx64)  
4. Run an update `sudo pacman -Syu`  
4. Reboot and repeat steps III) 1 to 3
5. Install [DMHacker's Jakeday Linux-Surface kernel fork](https://github.com/dmhacker/arch-linux-surface).  
6. Repeat steps III) 1 to 3  
7. Open a terminal and type  `efibootmgr`Look for the *Manjaro* and the *EFI USB* entries, take note of their ID (the 4-digit number on the left)  
8. For me, Manjaro = 0002, Windows = 0000, EFI USB = 2001 and EFI Network is 2002  
So I typed `efibootmgr -o 2001,0002,0000,2002` in order to have Manjaro be the first boot option  
9. Rebuild grub config file `grub-mkconfig -o /boot/grub/grub.cfg`  
10. **ENJOY!**  

------  


Hope this helps :)  


If I made a mistake or something is unclear, please gently let me know.  
Some people might like to use grub-customize, I don't.  
This is a simple guide, nothing specific to the Surface Go except the Kernel steps so you can use this for any Linux install with a little bit of tweaking.  

------

Special thanks to :
- [Jakeday](https://github.com/jakeday) Linux-Surface kernel
- [DMHacker](https://github.com/dmhacker) Linux-Surface kernel's Arch fork
- [Arch Wiki contributors](https://wiki.archlinux.org/index.php/GRUB#UEFI_systems) Archlinux's _"Bible"_   

------

Regards,  
Kay
