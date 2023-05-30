<h3 align="center">
  <a href="https://github.com/jevo1900/Linux-Things/blob/main/README.md">Menu</a>
</h3>

---

# Installing Arch linux with EFI

### Change keyboard layout:

   `loadkeys es`

### Verify boot mode:

   `ls /sys/firmware/efi/efivars` (If the directory exist your computer supports EFI)

### Ping some site on the Internet to verify connection:

   `ping archlinux.org`
   
### Update system clock:

   `timedatectl set-ntp true`
   
   You can verify the status with `timedatectl status`

### Enable SSH:

`systemctl start sshd`

### Change root password:

`passwd`

### (Optional) Go to [https://archlinux.org/mirrorlist](https://archlinux.org/mirrorlist) and find the closest mirror that supports HTTPS:

Add the mirrors on top of the `/etc/pacman.d/mirrorlist` file.

`Server = https://mirror.cloroformo.org/archlinux/$repo/os/$arch` (Spain)

### Partitioning the disk:

   #### enter into the disk:
   
   `fdisk /dev/sda`
   
   #### create a new partition table:
   
   `g`
   
   #### create a new partition:
   
   `n`
      
   this requires some parameters: first, the index, second, the initial sector, third, the ending sector,  the second and third parameters define the disk size.
    
   We will need some partitions to the system, an EFI partition, an root partition and a home partition.
      
   #### save the disk partition changes:
   
   `w`
      
### Create the filesystems:
    mkfs.fat -F32 /dev/sda1
    mkfs.ext4 /dev/sda2
    mkfs.ext4 /dev/sda3

### Create the `/root` and `/home` directories:
    mount /dev/sda2 /mnt
    mkdir /mnt/home
    mount /dev/sda3 /mnt/home

### Install Arch linux base packages:
    pacstrap -i /mnt base linux linux-firmware

### Generate the `/etc/fstab` file:
    genfstab -U -p /mnt >> /mnt/etc/fstab

### Chroot into installed system:
    arch-chroot /mnt

### Set the timezone:
    ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
    
### Set the keyboard layout with persistency: 
    localectl set-keymap --no-convert es

### Update the Hardware clock:
    hwclock --systohc
    
### Network Configurations
    pacman -S networkmanager
    systemctl enable NetworkManager

### Install boot manager and other needed packages:
    pacman -S grub efibootmgr dosfstools openssh os-prober mtools linux-headers linux-lts linux-lts-headers

### Set locale:
    sed -i 's/#es_ES.UTF-8/es_ES.UTF-8/g' /etc/locale.gen (uncomment es_ES.UTF-8)
    locale-gen

### Enable root login via SSH:
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
    systemctl enable sshd.service
    passwd` (for changing the root password)

### Create EFI boot directory:
    mkdir /boot/EFI
    mount /dev/sda1 /boot/EFI

### Install GRUB on EFI mode:
    grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck

### Setup locale for GRUB:
    cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

### Write GRUB config:
    grub-mkconfig -o /boot/grub/grub.cfg

### Create swap file:
    fallocate -l 2G /swapfile
    chmod 600 /swapfile
    mkswap /swapfile
    echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab
    
### Now you can create your user:
    useradd -m username
    passwd username
    usermod -aG wheel,video,audio,storage username

### In order to have root privileges we need sudo:
    pacman -S sudo

### Edit /etc/sudoers with nano or vim by uncommenting this line:
    Uncomment to allow members of group wheel to execute any command
    %wheel ALL=(ALL) ALL

### Exit, unount and reboot:
    exit
    umount -a
    reboot
