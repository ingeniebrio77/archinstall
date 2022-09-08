# Archlinux Installation VirtualBox 08/2022

### Fix PGP Schluessel

	[$EDITOR] /etc/pacman.conf
> SigLevel = Requeired DatabaseOptional to 

>> SigLevel = Never 

	sed -i 's/^SigLevel.*/SigLevel = Never/' /etc/pacman.conf
### Tastatur Layout aud Deutsch

	loadkeys de-latin1

	setfont ter-132n

### SSH Verbindung 

> Üeberpruefen ob SSH aktiv ist 

	systemctl status sshd

>> Password fuer User:

	passwd

### Mirror Aktualisierer

	reflector -c Germany -a 6 --sort rate --save /etc/pacman.d/mirrorlist

> Refresh Server

	pacman -Syy

### Partitionieren GPT oder MBR

	lsblk

####Partitionirungstool starten mit path/to/Festplatte

##### cfdisk, fdisk oder gdisk

	fdisk /dev/sda

> EFI:

* 300M=/dev/sda1
* 1G=/dev/sda2
* 50G=/dev/sda3
* Rest=/dev/sda4

### Formatieren der Partitionen
> EFI Format=FAT32

	mkfs.fat -F32 /dev/sda1

>> SWAP Formatieren und aktivieren

	mkswap /dev.sda2

	swapon /dev/sda2

>>> Root Partition EXT4 oder BTRFS

	mkfs.ext4 /dev/sda3

>>>> Home Paritition EXT4 oder BTRFS

	mkfs.ext4 /dev/sda4

>>>>> Überprüfen

	lsblk

### Partitionen Montieren

> / Partition in /mnt montieren

	mount /dev/sda3 /mnt

>> Verzeinischer erstellen für /boot/efi und /home    

	mkdir -p /mnt/{boot/efi,home}

>>> Montieren EFI Partition in dazugehörige Verzeichniss

	mount /dev/sda1 /mnt/boot/efi

>>>> Montieren /home    

	mount /dev/sda4 /mnt/home

>>>>> Überprüfen   

	lsblk

### Base Pakette

	pacstrap /mnt base linux linux-firmware git vim intel-ucode grub efibootmgr virtualbox-guest-utils lxde

### File Systemtable generieren

	genfstab -U /mnt >> /mnt/etc/fstab

### Root User in neuer Installation wechseln

	arch-chroot /mnt

### Time Zone

	ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime

### Zeit Synchronisieren

	hwclock --systohc

### Locales
	echo 'de_DE.UTF-8 UTF-8' >> /etc/locale.gen

	echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen

    	echo 'LANG=de_DE.UTF-8' >> /etc/locale.conf

    	echo "arch" >> /etc/hostname

	echo "127.0.0.1 localhost" >> /etc/hosts

    	echo "::1       localhost" >> /etc/hosts

    	echo "127.0.1.1 arch.localdomain arch" >> /etc/hosts

### Root Password Aendern

    passwd

### Extras Paketten

	pacman -S sudo grub efibootmgr networkmanager network-manager-applet dialog wpa_supplicant mtools dosfstools base-devel linux-headers avahi xdg-user-dirs xdg-utils gvfs gvfs-smb nfs-utils inetutils dnsutils bluez bluez-utils cups hplip alsa-utils pipewire pipewire-alsa pipewire-pulse pipewire-jack bash-completion openssh rsync reflector acpi acpi_call tlp virt-manager qemu qemu-arch-extra edk2-ovmf bridge-utils dnsmasq vde2 openbsd-netcat ipset firewalld flatpak sof-firmware nss-mdns acpid os-prober ntfs-3g terminus-font

### Grub Installation

	grub-install

	grub-mkconfig -o /boot/grub/grub.cfg

### Dienste Starten

	systemctl enable NetworkManager

	systemctl enable bluetooth

	systemctl enable cups.service

	systemctl enable sshd

	systemctl enable avahi-daemon

	systemctl enable tlp

	systemctl enable reflector.timer

	systemctl enable fstrim.timer

	systemctl enable libvirtd

	systemctl enable firewalld

	systemctl enable acpid

	systemctl enable lxdm

	systemctl enable vboxservice

### Neu User erstellen

	useradd -mG wheel renato

	passwd renato

> visudo, falls vim und sudo installiert ist, sonst $EDITOR /etc/sudoers
>> Auskommentieren

	%wheel ALL=(ALL)ALL

### Neustarten
	exit

	umount -a

	poweroff

####ISO Datei enfertnen

### XFCE$ DEsktop Install 
	sudo pacman -S --needed xorg

###     Install additional components and applications (approx 144 MB)

	sudo pacman -S --needed xfce4 mousepad parole ristretto thunar-archive-plugin thunar-media-tags-plugin xfce4-battery-plugin xfce4-datetime-plugin xfce4-mount-plugin xfce4-netload-plugin xfce4-notifyd xfce4-pulseaudio-plugin xfce4-screensaver xfce4-taskmanager xfce4-wavelan-plugin xfce4-weather-plugin xfce4-whiskermenu-plugin xfce4-xkb-plugin file-roller network-manager-applet leafpad epdfview galculator lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings capitaine-cursors arc-gtk-theme xdg-user-dirs-gtk

### Extras Packages

	sudo pacman -S --needed xfce4-goodies file-roller network-manager-applet leafpad epdfview galculator lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings capitaine-cursors arc-gtk-theme xdg-user-dirs-gtk

### Extras für Virtuelle Machinen

	sudo pacman -S --needed virtualbox-guest-utils xf86-video-vmware

### Activate Display Manager (light-dm)

	sudo systemctl enable lightdm

### System neu starten

	reboot

> Ende
