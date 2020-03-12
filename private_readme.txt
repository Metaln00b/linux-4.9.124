sudo apt-get install gcc-arm-*
apt-get install rsync
wget https://www.kernel.org/pub/linux/kernel/v4.x/linux-4.9.124.tar.xz
tar xvfJ linux-4.9.124.tar.xz
cd linux-4.9.124
git init; git add .; git commit -m "Linux vanilla"; git branch acme; git checkout acme
wget https://raw.githubusercontent.com/AcmeSystems/acmepatches/master/linux-4.9.patch
patch -p1 < linux-4.9.patch
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- acme-foxg20_defconfig
make ARCH=arm menuconfig */ Optional */
make ARCH=arm savedefconfig
cp defconfig arch/arm/configs/myboard_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- acme-foxg20.dtb

Compile the Linux Kernel sources and generate the binary compressed image file to save in the first partition of microSD card.
make -j8 ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- zImage
make modules -j8 ARCH=arm CROSS_COMPILE=arm-linux-gnueabi-
make modules_install INSTALL_MOD_PATH=./modules ARCH=arm

Write the Linux Kernel image, the Device tree blog files in the first microSD partition and uncompress the modules in /modules/lib directory inside the second microSD partition:

cat arch/arm/boot/zImage arch/arm/boot/dts/acme-foxg20.dtb > /media/$USER/BOOT/uImage
sudo rsync -avc modules/lib/. /media/$USER/rootfs/lib/.

At the first access to the board command line update the module dependencies by typing this command:

depmod -a

