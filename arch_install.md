### Arch linux install manual

timedatectl set-ntp true  
cfdisk /dev/sda:  (choose dos)   

/dev/sda1 (256M type Linux bootable)  
/dev/sda2 (all other space type Linux)  
white-yes then Quit  

mkfs.ext4 /dev/sda1  
mkfs.ext4 /dev/sda2  
mount /dev/sda2 /mnt  
mkdir /mnt/boot  
mount /dev/sda1 /mnt/boot  

vim /etc/pacman.d/mirrorlist   (add ru mirror to the top)  
pacstrap /mnt base base-devel linux linux-firmware vim  
genfstab -U /mnt >> /mnt/etc/fstab  
arch-chroot /mnt /bin/bash  

pacman -S networkmanager grub  
systemctl enable networkmanager  
grub-install /dev/sda  
grub-mkconfig -o /boot/grub/grub.cfg  
passwd   (set root password)  
vim /etc/locale.gen  (add en and ru utf8 locales)  
locale-gen  
vim /etc/locale.conf #LANG=en_US.UTF-8  
vim /etc/hostname   
ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime  
exit  
umount -R /mnt  
reboot  

pacman -Syy  
pacman -S nvidia nvidia-utils  (install nvidia drivers or another)  
pacman -S xorg xfce4 xfce4-goodies lightdm lightdm-gtk-greeter  (install xfce)  
sudo systemctl enable lightdm  
reboot  

