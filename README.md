# rpi3kernel_bestsuit
this is a repo to rpi3 linux kernel for everyone easy to read
we have purpose, most of time, many of us are really confused on reading linux kernel source code. 
it really a pain of ass! too many files which are not even compiled. too many configrations. so we
decide to delete all the unuseful files in from the kernel with rpi3 board.

step 1 :
we have chosen a version of rpi3 kernel(raspberrypi-kernel_1.20210430-1  from https://github.com/raspberrypi/linux/tags)
we downloaded linux-raspberrypi-kernel_1.20210430-1.tar.gz.

tar zxvf linux-raspberrypi-kernel_1.20210430-1.tar.gz
git add linux-raspberrypi-kernel_1.20210430-1

step 2 :
setup a cross compiler enviroment,
1) Install required dependencies and toolchain 
sudo apt install git bc bison flex libssl-dev make libc6-dev libncurses5-dev
2) Install the 64-bit toolchain for a 64-bit kernel
sudo apt install crossbuild-essential-arm64


step 3 :
at least we have to know how to cross compile kernel. here is some reference
for 64-bit configs to Pi 3, Pi 3+ or Compute Module 3:

#build configration file for board only
cd linux-raspberrypi-kernel_1.20210430-1
KERNEL=kernel8
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bcmrpi3_defconfig


step 4 :
Customising the Kernel version using LOCALVERSION
In addition to your kernel configuration changes, you may wish to adjust the LOCALVERSION to ensure your new kernel does not receive the same version string as the upstream kernel. This both clarifies you are running your own kernel in the output of uname and ensures existing modules in /lib/modules are not overwritten.

To do so, change the following line in .config:

CONFIG_LOCALVERSION="-v7l-MY_CUSTOM_KERNEL"

step 5 :
#build with more configs
For all 64-bit builds
Note: Note the difference between Image target between 32 and 64-bit.

make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image modules dtbs


step 6 :
here is a example to install kernel module *ko etc
sudo env PATH=$PATH make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- INSTALL_MOD_PATH=/home/zylinux/Projects_min/raspberrypi3/software/kernel/test modules_install


step 7 :
sudo cp /boot/$KERNEL.img /boot/$KERNEL-backup.img
sudo cp arch/arm64/boot/Image /boot/$KERNEL.img
sudo cp arch/arm64/boot/dts/broadcom/*.dtb /boot/
sudo cp arch/arm64/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm64/boot/dts/overlays/README /boot/overlays/

scp arch/arm64/boot/Image pi@192.168.1.127:/home/pi/test
scp arch/arm64/boot/dts/broadcom/*.dtb pi@192.168.1.127:/home/pi/test
scp arch/arm64/boot/dts/overlays/*.dtb* pi@192.168.1.127:/home/pi/test
scp arch/arm64/boot/dts/overlays/README pi@192.168.1.127:/home/pi/test


step 8 :
there is a config file witch is /boot/config.txt 
you could specify kernel=kernel-myconfig.img to choose the right one for you.


