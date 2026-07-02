gentoo.org
links mirrors.ustc.edu.cn/gentoo

## Installing Gentoo
  https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage

## About the Gentoo Linux installation
  https://www.bilibili.com/opus/1002694814294081584
  https://www.bilibili.com/video/BV1zFx5e8EbZ
  https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation
## Choosing the right installation medium
  Minimal Installation CD
    https://distfiles.gentoo.org/releases/amd64/autobuilds/20260531T160106Z/install-amd64-minimal-20260531T160106Z.iso
  LiveGUI USB Image
    https://distfiles.gentoo.org/releases/amd64/autobuilds/20260510T170106Z/livegui-amd64-20260510T170106Z.iso
  stage3-amd64-desktop-systemd
    https://distfiles.gentoo.org/releases/amd64/autobuilds/20260621T164603Z/stage3-amd64-desktop-systemd-20260621T164603Z.tar.xz
    https://distfiles.gentoo.org/releases/amd64/autobuilds/20260621T164603Z/stage3-amd64-desktop-systemd-20260621T164603Z.tar.xz
    https://mirrors.ustc.edu.cn/gentoo/releases/amd64/autobuilds/20260621T164603Z/stage3-amd64-desktop-systemd-20260621T164603Z.tar.xz
    https://mirrors.ustc.edu.cn/gentoo/releases/amd64/autobuilds/20260621T164603Z/stage3-amd64-desktop-systemd-20260621T164603Z.tar.xz.sha256
    https://mirrors.aliyun.com/gentoo/releases/amd64/autobuilds/20260621T164603Z/
  qcow2 no root pw | no multilib | systemd
    https://distfiles.gentoo.org/releases/amd64/autobuilds/20260621T164603Z/di-amd64-console-20260621T164603Z.qcow2
    https://mirrors.ustc.edu.cn/gentoo/releases/amd64/autobuilds/20260621T164603Z/di-amd64-console-20260621T164603Z.qcow2
    https://mirrors.ustc.edu.cn/gentoo/releases/amd64/autobuilds/20260621T164603Z/di-amd64-console-20260621T164603Z.qcow2.sha256

## Configuring the network
ping
net-setup
passwd root
rc-service sshd start
ip a
ssh root@ip

## Preparing the disks
lsblk -f
cfdisk /dev/sda
 gpt
  uefi 1G
  swap 4G
  ext4
  
mkfs.fat -F 32 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3

mount /dev/sda1 /mnt/gentoo/efi --mkdir
swapon /dev/sda2
mount /dev/sda3 /mnt/gentoo

cd /mnt/gentoo/


## Installing the Gentoo installation files
chronyd -q

nano /etc/resolv.conf
  nameserver 1.1.1.1

### Choosing a stage file  
links https://www.gentoo.org/downloads/mirrors/
links https://mirrors.ustc.edu.cn/gentoo
stage3-amd64-desktop-openrc-20260621T164603Z.tar.xz
stage3-amd64-desktop-systemd-20260621T164603Z.tar.xz

### Installing a stage file
tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner -C /mnt/gentoo

### Configuring compile options
nano /mnt/gentoo/etc/portage/make.conf
Compiler flags to set for all languages
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"

nano /etc/portage/make.conf
/* EMERGE_DEFAULT_OPTS is set automatically by livecd-tools autoconfig during first live boot.
/* This should be equal to number of processors, see "man emerge" for details.
EMERGE_DEFAULT_OPTS="${EMERGE_DEFAULT_OPTS} --jobs=2 --load-average=2"

## Installing the Gentoo base system
cat /etc/resolv.conf
cat /mnt/gentoo/etc/resolv.conf
cp --dereference /etc/resolv.conf /mnt/gentoo/etc/

mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
mount --bind /run /mnt/gentoo/run
mount --make-slave /mnt/gentoo/run 

test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm 

chmod 1777 /dev/shm /run/shm

chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"

/* arch-chroot /mnt/gentoo
/* export PS1="(chroot) ${PS1}"

mkdir /efi

mount /dev/sda1 /efi
/* mount /dev/vda1 /boot

emerge --ask --verbose --oneshot app-portage/mirrorselect
mirrorselect -i -o >> /etc/portage/make.conf

echo emerge --sync
emerge-webrsync


## Configuring the Linux kernel

eselect profile list

eselect profile set 5

getuto

portageq envvar ACCEPT_LICENSE

mkdir /etc/portage/package.license

emerge --ask --verbose --update --deep --changed-use @world

emerge --ask --verbose --update --deep --newuse --getbinpkg @world

emerge --ask --pretend --depclean

emerge --ask --depclean

ls -l /usr/share/zoneinfo/
ls -l /usr/share/zoneinfo/Asia/
ln -sf ../usr/share/zoneinfo/Asia/Shanghai /etc/localtime

nano /etc/locale.gen
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8

locale-gen

eselect locale list

eselect locale set 2

env-update && source /etc/profile && export PS1="(chroot) ${PS1}"

## Configuring the system
emerge --ask sys-kernel/linux-firmware

emerge --ask sys-firmware/sof-firmware

emerge --ask sys-kernel/installkernel


emerge --ask sys-apps/systemd-utils

mkdir -p /etc/cmdline.d
ln -s /etc/kernel/cmdline /etc/cmdline.d/00-installkernel.conf

echo For OpenRC systems 
emerge --ask sys-kernel/installkernel

mkdir -p /efi/EFI/Gentoo

mkdir /etc/dracut.conf.d
blkid

emerge --ask sys-kernel/installkernel

emerge --ask sys-apps/systemd

emerge --ask sys-apps/systemd-utils

emerge --ask sys-kernel/installkernel

emerge --ask sys-kernel/installkernel

echo 配置系统

emerge -a genfstab
genfstab -U / >> /etc/fstab

nano /etc/fstab

echo 网络信息
echo 主机名
echo 设置主机名（OpenRC 和 systemd）
echo tux > /etc/hostname
echo 主机名 systemd 
hostnamectl hostname tux

echo 网络
echo 网络安装
emerge --ask net-misc/dhcpcd
echo 网络openrc
rc-update add dhcpcd default
rc-service dhcpcd start 
echo 网络systemd
echo systemctl enable dhcpcd

echo emerge --ask --noreplace net-misc/netifrc
echo nano /etc/conf.d/net

## Installing system tools
echo 安装工具
emerge --ask app-admin/sysklogd
rc-update add sysklogd default

rc-update add sshd default

emerge --ask app-shells/bash-completion

emerge --ask net-misc/chrony
rc-update add chronyd default

## Configuring the bootloader

配置引导加载程序
#emerge --ask --verbose sys-boot/grub
echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf
emerge --ask sys-boot/grub
#emerge --ask --update --newuse --verbose sys-boot/grub
grub-install --target=x86_64-efi --efi-directory=/efi

#opt secureboot
emerge sys-boot/grub sys-boot/shim sys-boot/mokutil sys-boot/efibootmgr
cp /usr/share/shim/BOOTX64.EFI /efi/EFI/Gentoo/shimx64.efi
cp /usr/share/shim/mmx64.efi /efi/EFI/Gentoo/mmx64.efi
cp /usr/lib/grub/grub-x86_64.efi.signed /efi/EFI/Gentoo/grubx64.efi 
openssl x509 -in /path/to/kernel_key.pem -inform PEM -out /path/to/kernel_key.der -outform DER

## Finalizing the installation
exit

cd
umount -l /mnt/gentoo/dev{/shm,/pts,}
umount -R /mnt/gentoo
reboot


## Finalizing the installation

#
https://github.com/microsoft/azurelinux
