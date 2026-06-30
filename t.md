gentoo.org
links mirrors.ustc.edu.cn/gentoo

## Installing Gentoo
  https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage

## About the Gentoo Linux installation
  https://www.bilibili.com/opus/1002694814294081584
  https://www.bilibili.com/video/BV1zFx5e8EbZ

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
  
# links https://www.gentoo.org/downloads/mirrors/
links https://mirrors.ustc.edu.cn/gentoo
stage3-amd64-desktop-systemd-20260621T164603Z.tar.xz
stage3-amd64-desktop-openrc-20260621T164603Z.tar.xz

tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner -C /mnt/gentoo

## Installing the Gentoo base system
nano /mnt/gentoo/etc/portage/make.conf
Compiler flags to set for all languages
COMMON_FLAGS="-march=native -O2 -pipe"
# Use the same settings for both variables
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"

nano /etc/portage/make.conf
/* EMERGE_DEFAULT_OPTS is set automatically by livecd-tools autoconfig during first live boot.
/* This should be equal to number of processors, see "man emerge" for details.
EMERGE_DEFAULT_OPTS="${EMERGE_DEFAULT_OPTS} --jobs=2 --load-average=2"

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

## Configuring the Linux kernel
chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"

/* arch-chroot /mnt/gentoo
/* export PS1="(chroot) ${PS1}"

mkdir /efi

mount /dev/sda1 /efi
/* mount /dev/vda1 /boot


emerge --sync
emerge --ask --verbose --oneshot app-portage/mirrorselect
mirrorselect -i -o >> /etc/portage/make.conf

## Configuring the system

## Installing system tools

## Configuring the bootloader

## Finalizing the installation
    



emerge-webrsync

eselect profile list

eselect profile set 5

getuto

portageq envvar ACCEPT_LICENSE

mkdir /etc/portage/package.license

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

emerge --ask sys-kernel/linux-firmware

emerge --ask sys-firmware/sof-firmware

emerge --ask sys-kernel/installkernel

emerge --ask sys-apps/systemd-utils

mkdir -p /etc/cmdline.d
ln -s /etc/kernel/cmdline /etc/cmdline.d/00-installkernel.conf

emerge --ask sys-kernel/installkernel

mkdir -p /efi/EFI/Gentoo

mkdir /etc/dracut.conf.d
blkid

emerge --ask sys-kernel/installkernel

emerge --ask sys-apps/systemd

emerge --ask sys-apps/systemd-utils

emerge --ask sys-kernel/installkernel

emerge --ask sys-kernel/installkernel

配置系统

emerge -a genfstab
genfstab -U / >> /etc/fstab

nano /etc/fstab

BIOS 系统
/dev/sda1   /boot        xfs    defaults    0 2
/dev/sda2   none         swap    sw                   0 0
/dev/sda3   /            xfs    defaults,noatime              0 1
</div>
/dev/cdrom  /mnt/cdrom   auto    noauto,user          0 0

UEFI 系统
# Adjust for any formatting differences and/or additional partitions created from the "Preparing the disks" step
/dev/sda1   /efi        vfat    umask=0077,tz=UTC     0 2
/dev/sda2   none         swap    sw                   0 0
/dev/sda3   /            xfs    defaults,noatime              0 1
</div>
<div lang="en" dir="ltr" class="mw-content-ltr">
/dev/cdrom  /mnt/cdrom   auto    noauto,user          0 0

DPS UEFI PARTUUID
# Adjust any formatting difference and additional partitions created from the "Preparing the disks" step.
# This example shows a GPT disklabel with Discoverable Partition Specification (DPS) UUID set:
PARTUUID=c12a7328-f81f-11d2-ba4b-00a0c93ec93b   /efi        vfat    umask=0077,tz=UTC            0 2
PARTUUID=0657fd6d-a4ab-43c4-84e5-0933c84b4f4f   none        swap    sw                           0 0
PARTUUID=4f68bce3-e8cd-4db1-96e7-fbcaf984b709   /           xfs     defaults,noatime             0 1

网络信息
主机名
设置主机名（OpenRC 和 systemd）
echo tux > /etc/hostname
主机名 systemd 
hostnamectl hostname tux

网络
网络安装
emerge --ask net-misc/dhcpcd
网络openrc
rc-update add dhcpcd default
rc-service dhcpcd start 
网络systemd
systemctl enable dhcpcd

emerge --ask --noreplace net-misc/netifrc
nano /etc/conf.d/net

安装工具
emerge --ask app-admin/sysklogd
rc-update add sysklogd default

rc-update add sshd default

emerge --ask app-shells/bash-completion

emerge --ask net-misc/chrony
rc-update add chronyd default

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

exit

cd
umount -l /mnt/gentoo/dev{/shm,/pts,}
umount -R /mnt/gentoo
reboot
