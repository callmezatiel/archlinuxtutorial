Primero queda establecer tu distribucion de teclado en mi caso es "Teclado Latinoamericano"

```
loadkeys la-latin1
```

Seguido de establecer el valor de fecha y hora | Muy importante para que los repositorios funcionen 
```
timedatectl set-ntp true
```

Definimos el espacio en el disco duro esto queda a consideracion de el usuario yo considero que esto es lo mejor, tomando en cuenta el uso que se le da de acuerdo a los tutoriales del canal
```
cfdisk
```

sda1 512MB EFI
sda2 6GB   SWAP
sda3 *     EXT4 boot

Listamos y establecemos el formato de cada particion 
```
lsblk
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2
mkfs.btrfs /dev/sda3
```

Procedemos a montar las particiones
```
mount /dev/sda3 /mnt
```

Listamos para confirmar
```
ls 
```
Listamos para verificar lo que hay en raiz
```
ls /
```
Creamos un directorio y establecemos las unidades
```
mkdir /mnt/boot
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
```

Verificamos y establecemos el mirror en el sistema
```
nano /etc/pacman.d/mirrorlist
```

Se establecen utilidades varias, sistema base y fstab
```
pacstrap /mnt base base-devel linux linux-firmware nano
genfstab -U  /mnt >> /mnt/etc/fstab
```

Verificamos
```
nano /mnt/etc/fstab
```


arch-chroot /mnt

Generamos y establecemos informacion adicional en mi caso usare "en_US.UTF-8 UTF-8" en locale.gen
```
ln -sf /usr/share/zoneinfo/Canada/Eastern /etc/localtime
hwclock --systohc
nano /etc/locale.gen        
```              

Establecemos valores adicionales
```                     
echo LANG=en_US.UTF-8 > /etc/locale.conf
echo KEYMAP=la-latin1 > /etc/vconsole.conf
echo NatyZaty > /etc/hostname
en_US.UTF-8 UTF-8
```

Editamos y asignamos el nombre al equipo
```
nano /etc/hosts
```


127.0.0.1            localhost
::1                  localhost
127.0.0.1            Archlinux.localdomain    Archlinux


Instalamos driver de red y lo habilitamos
```
pacman -S networkmanager
systemctl enable NetworkManager
```

Asignamos contraseña
```
passwd
```
Instalamos grub segun sea el caso (UEFI / BIOS Legacy)

``` 
pacman -S grub efibootmgr
grub--install --target=x86_64-efi --efi-directory=boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
```

Desmontamos 
```
unmount -R /mnt
```

Salimos                                        
```
exit
```

Reiniciamos y listo 
```
reboot
```
