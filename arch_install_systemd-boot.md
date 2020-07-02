### Arch linux install manual with systemd-boot  

timedatectl set-ntp true  

gdisk /dev/nvme0n1 (press x, y, enter x2)  
cfdisk:  
  /dev/nvmen1p1 - 512M type EFI System  
  /dev/nvmen1p2 - all other type Linux root (x86-64)  
  
mkfs.fat -F32 /dev/nvme0n1p1  
mkfs.ext4 /dev/nvme0n1p2  
mount /dev/nvme0n1p2 /mnt  
mkdir /mnt/boot  
mount /dev/nvme0n1p1 /mnt/boot  

vim /etc/pacman.d/mirrorlist   (add ru mirror to the top)  
pacstrap -i /mnt base base-devel vim  
genfstab -U /mnt >> /mnt/etc/fstab   
arch-chroot /mnt /bin/bash  

vim /etc/locale.gen  (add en and ru utf8 locales)  
locale-gen  
echo LANG=en_US.UTF-8 > /etc/locale.conf  
export LANG=en_US.UTF-8  
ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime  
hwclock --systohc  
echo archlinux > /etc/hostname  
passwd   (set root password) 
pacman -S dhcpcd  
systemctl enable dhcpcd  

bootctl install  
vim /boot/loader/loader.conf  
   timeout 3  
   default arch  
vim /boot/loader/entries/arch.conf  
   title Arch Linux  
   linux /vmlinuz-linux  
   initrd /initramfs-linux.img  
   options  root=/dev/nvme0n1p2 rw  
   
exit  
umount -R /mnt/boot  
umount -R /mnt  
reboot  

pacman -Syu  
pacman -S bash-completion  
pacman -S xorg xfce4 xfce4-goodies lightdm lightdm-gtk-greeter  (install xfce & intel drv)  
sudo systemctl enable lightdm  
reboot  

