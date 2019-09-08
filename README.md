# dualbootfix
Fix bootloader for systems facing trouble while dual booting. Especially for HP users.
######################

People having trouble loading Grub / rEFInd as default bootloader can make  use of this.

 I could've made an executable file for this,but it would mess up some things. So I skipped it. 

People having Windows and *nix as dual boot could follow this. I'm more intentionally making this for HP users having UEFI BIOS. But others can also try this, it will possibly work. You should have either installed efibootmgr. Then, Grub or rEFInd . 

Possibly, /dev/sda1 will be the boot partition with a FAT32 format around 200-350 MB. It contains Boot files and our .efi files. You can verify whether this is your boot partition by mounting it in your current working dir.

Mkdir bootsearch 
mount /dev/sda1 /bootsearch
While creating boot partition, it could be in /efi or /efi/Boot. Probably it's in /efi

If you have already installed Grub or rEFInd. It could be in /efi/EFI/Boot/bootx64.efi 
(Note, above directory path is independent and depends on where you installed grub or rEFInd)

After you find the bootloader folder, it's time for us to add it to boot entry of efibootmgr.

efibootmgr -c -d /dev/sda -p 1 -l \\efi\\EFI\\Boot\\grubx64.efi -L anynameforentry 

Where,  
-c | --createCreate new variable bootnum and add to bootorder

-d | --disk DISK The disk containing the loader (defaults to /dev/sda)

-p | --part PART Partition number containing the bootloader (defaults to 1)

Specify a loader (defaults to \\elilo.efi)-L | --label LABELBoot manager display label (defaults to "Linux")

After adding the boot entry, just issue
efibootmgr command again and put our anynameforentry at the top by

efibootmgr -o 0004,0000,0002

I'm assuming 0004  is our anynameforentry and 0000 is Windows entry number. Do correctly check!

Now everything is done. You should now shutdown the system.

sudo shutdown now

Now, don't boot the system, instead , go-to the BIOS. Look for the boot order tab and make Arch(anynameforentry) at the top of the list. Save settings with F10 key. Before leaving the BIOS, press F10 to save all settings that we made. Done!
Now you'll find your Grub/rEFInd after booting )


