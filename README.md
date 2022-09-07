# Archlinux Installation (VirtualBox)

### Tastatur Layout
	loadkeys de-latin1
### erstes Bild
![Alt-text](Bilder/git_repository.png)

### Überprüfen wie die Festplatte heißt

	lsblk
### Datenbank refresh
	pacman -Syy
### Mirros aktualisieren (reflector)
	reflector -c Germany -a 6 --sort rate --save /etc/pacman.d/mirrorlist
### Datenbank refresh
	pacman -Syy
### Partitionierungstools starten
	cfdisk /dev/sda
gpt Partitionstabelle erstellen
1. 300MB >> EFI >> EFI System
2. 4GB >> SWAP >> Linux Swap
3. 30GB >> / >> LinuxFileSystem
4. Rest >> /home >> LinuxFileSystem
5. Write >> Quit

	lsblk
### Partitionsformat
#### EFI Format FAT 32
	mkfs.fat -F32 /dev/sda1
#### SWAP Format
	mkswap /dev/sda2
#### SWAP aktivieren
	swapon /dev/sda2
#### Root Format ext4
	mkfs.ext4 /dev/sda3
#### /home Format ext4
	mkfs.ext4 /dev/sda4
