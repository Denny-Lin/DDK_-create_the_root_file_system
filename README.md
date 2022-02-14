# DDK_create_the_root_file_system

```C
1. make directories for the root file system
    mkdir ~/rootfs
    cd ~/rootfs
    mkdir bin dev etc home lib proc sbin sys tmp usr var
    mkdir usr/bin usr/lib usr/sbin
    mkdir var/log

    tree -d

    .
    ├── bin
    ├── dev
    ├── etc
    ├── home
    ├── lib
    ├── proc
    ├── sbin
    ├── sys
    ├── tmp
    ├── usr
    │   ├── bin
    │   ├── lib
    │   └── sbin
    └── var
        └── log
 
2.Download and Install busybox
    cd ~/Desktop/
    git clone git://busybox.net/busybox.git
    cd busybox
    git checkout 1_24_1

    make distclean
    make defconfig

    static tools
    make menuconfig
    Settings  --->                                                                     
    [*] Build static binary (no shared libs)  

    make -j 4 ARCH=arm CRSS_COMPILE=arm-cortex_a8-linux-gnueabi-

3.Make tools for the root file system
    make CRSS_COMPILE=arm-cortex_a8-linux-gnueabi- install

    tools path: /home/denny/Desktop/busybox/_install/

4.Copy tools into the root file system
    cp -r -a /home/denny/Desktop/busybox/_install/* /home/denny/rootfs

5.Copy kernel modules if you have them

6.Make device nodes for busybox
    cd ~/rootfs
    sudo mknod -m 666 dev/null c 1 3
    sudo mknod -m 660 dev/console c 5 1

7.Create the root file system  
cd ~/rootfs
find . | cpio -H newc -ov --owner root:root > ../initramfs.cpio
cd ..
sudo gzip initramfs.cpio
mkimage -A arm -O linux -T ramdisk -d initramfs.cpio.gz uRamdisk

```

... </br>
