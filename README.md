# SIMTEK Service System
The utility OS (based on arch, btw, on Linux 6.3) created by me for use on client computers


## Included tools
- beep
- curl
- efibootmgr
- efivar
- fuse2
- fdisk
- gzip
- hdparm
- hwdata
- iproute2
- iptables
- iputils
- keyutils
- lz4
- ncurses
- networkmanager
- ntfs-3g
- openssl
- pciutils
- pv
- tar
- tzdata
- vi
- vim


## Default Login Information
Because the OS is pre-packaged, it already has a user account created. 

Default root password: "password"

Default Username: "servicesystem"
Default User Password: "password"

PLEASE MAKE SURE TO CHANGE THESE PASSWORDS

NOTE: Performing ANY needed commands and utilities in this OS as root is NOT supported. It may cause damage and/or data loss.


## Installation Instructions
This OS is meant to be manually installed. A great tool to do so is an arch linux install LiveUSB. You can get an ISO at https://archlinux.org/download/

If you are installing from a release, partition the drive as you see fit. The recommended boot loader is grub, and the tarball containing the filesystem already has the needed tools to install grub. You can use whatever boot loader you like.

The GUI release contains XFCE, while the Non-GUI contains no installed desktop environment.

1. Create an Arch Linux install USB (if you don't have one already)
2. Partition the drive as you see fit (If you don't know what the partition table should look like, take a look at Partitioning the Drive)
3. Mount the partitions, with the root partition of the new system at /mnt, and the efi partition to /mnt/boot (if applicable).
4. Download the desired release file to somewhere on the LiveUSB's filesystem
5. Extract the tarball to /mnt
6. Run "genfstab -U /mnt >> /mnt/etc/fstab" to generate the fstab based on the UUIDs of the partitions
7. arch-chroot into /mnt (arch-install-scripts required)
8. Run "mkinitcpio -P" to generate the initramfs
9. Install the desired bootloader (grub recommended, notes in Partitioning the Drive)
10. Create the grub configuration file (if using grub)
11. Reinstall sudo using "pacman -S sudo"
12. Change Default Passwords


## Partitioning the Drive
Make sure you're partitioning your targed drive, not the LiveUSB!

Fdisk works well to partition the drive. If you are going to be targeting your install towards UEFI-only systems, you just need the root partition and an EFI system partition. The EFI partition needs to be at least 300MiB. If you are targeting your install towards BIOS-only systems, you don't need an EFI partition. 

If you want to use both, it is suggested that you use a GUID Partition Table, making the first partition on the drive a "BIOS Boot Partition" with a size of 1MiB. Your second partition should be your 300MiB EFI partition, and your third partition should be your root partition. When installing grub for both BIOS and UEFI, make sure to add the --recheck and --removable options to your grub-install command. 
NOTE: --removable is only needed for the x86_64-efi target. It will not work for the i386-pc target. Instructions for dual-platform are as follows:

1. Run "fdisk /dev/sdX" (where X is the number for your target drive)
2. Type "g" for GUID Partition Table
3. Type "n" for New Partition, then press return twice, then type "+1M" for 1MiB.
4. Type "t", and then "4" to set the partition type to BIOS Boot.
5. Type "n" for new partition, press return twice, and type "+300M" for 300MiB.
6. Type "t", and then hit return, and then type "1" for EFI System Partition.
7. Type "n" for new partition, and press return three times to take up the rest of the drive's space.
8. Type "w" to write the changes to the disk. 
9. Create the ext4 filesystem for the root partition using "mkfs.ext4 /dev/sdX3", where X is the target device. 
10. Create the FAT32 filesystem for the EFI partition using "mkfs.fat -F 32 /dev/sdX2"
11. Mount the root partition to /mnt
12. Run "mkdir /mnt/boot" to create the mount point for the EFI partition
13. Mount the EFI partition to /mnt/boot
14. Extract the release tarball to /mnt
15. Generate the target fstab with "genfstab -U /mnt >> /mnt/etc/fstab"
16. arch-chroot into /mnt
17. Run "mkinitcpio -P" to generate the initramfs
18. Run "grub-install --target=i386-pc --recheck /dev/sdX" to install the legacy bootloader
19. Run "grub-install --target=x86_64-efi --efi-directory=/boot --recheck --removable" to install the EFI bootloader
20. Run "grub-mkconfig -o /boot/grub/grub.cfg" to generate the GRUB config file.
21. Run "pacman -S sudo" to reinstall sudo, repairing file permissions required for sudo to work.
22. Run "passwd" and set a new password
23. Reboot the system, removing the arch linux install USB
24. Login with "servicesystem" and "password"
25. Run "sudo passwd servicesystem" and set a new password for the user account


THIS SOFTWARE IS FREELY DISTRIBUTED. PLEASE DO WHATEVER YOU LIKE TO IT. THE FILESYSTEM IS AN UNCOMPRESSED TARBALL TO PRESERVE FILE PERMISSIONS. ISSUES AND FORKS ARE WELCOME.