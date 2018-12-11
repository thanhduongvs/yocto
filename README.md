# Yocto NFS & TFTP boot

![Build Status](https://img.shields.io/badge/yocto-NFS%20and%20TFTP%20boot-green.svg)

## giao thức truyền boot
![boot](https://github.com/thanhduongvs/yocto/blob/master/01_nfs-tftp-boot.png)

## host (ubuntu desktop)
1. Cài đặt TFTP server:  
- :computer: *$* `sudo apt-get install tftpd-hpa`
- :computer: *$* `sudo mkdir /tftpboot`
- :computer: *$* `sudo chmod 1777 /tftpboot`
- :computer: *$* `sudo gedit /etc/default/tftpd-hpa`
- :pencil: Chỉnh sửa file **/etc/default/tftpd-hpa** như bên dưới:
    ```
    TFTP_USERNAME="tftp"
    TFTP_DIRECTORY="/tftpboot"
    TFTP_ADDRESS="0.0.0.0:69"
    TFTP_OPTIONS="--secure"
    ```
- :computer: *$* `sudo service tftpd-hpa restart`

2. Cài đặt NFS server:
- :computer: *$* `sudo apt-get install nfs-kernel-server`
- :computer: *$* `sudo mkdir /nfsroot`
- :computer: *$* `sudo gedit /etc/exports`
- :pencil: Chỉnh sửa file **/etc/exports** như bên dưới:
    ```
    /nfsroot/ *(rw,no_root_squash,async,no_subtree_check)
    ```
- :computer: *$* `sudo service nfs-kernel-server restart`

3. Set static IP Ubuntu
- :computer: *$* `sudo gedit /etc/network/interfaces`
- :pencil: Chỉnh sửa file **/etc/network/interfaces** như bên dưới:
    ```bash
    auto enp0s25
    iface enp0s25 inet static
    address 192.168.0.10      <-- Your HOST IP
    netmask 255.255.255.0
    gateway 192.168.0.1
    ```
- :computer: *$* `sudo ifdown enp0s25`
- :computer: *$* `sudo ifup enp0s25`
- :computer: *$* `ifconfig`

4. build yocto
- :pushpin: Vào thư mục **yocto udoo neo**. Mở file *udoo-neo-bsp/source/meta-udoo/conf/machine/include/udoo-imx-base.inc* . Sửa dòng `IMAGE_FSTYPES = "wic.bz2"` thành `IMAGE_FSTYPES = "wic.bz2 tar.bz2"`
- :pushpin: Vào thư mục **udoo-neo-bsp** build lại image:
- :computer: *udoo-neo-bsp$* `MACHINE=udoovn DISTRO=fslc-framebuffer source setup-environment build`
- :computer: *udoo-neo-bsp/build$* `bitbake -k udoo-image-qt5`
- :pushpin: Chờ build xong thực hiện bước bên dưới
- *yocto$* `sudo cp build/tmp/deploy/images/udoovn/zImage-udoovn.bin /tftpboot`
- *yocto$* `sudo cp build/tmp/deploy/images/udoovn/zImage-udoovn.dtb /tftpboot`
- *yocto$* `sudo tar xvfp build/tmp/deploy/images/udoovn/udoo-image-qt5-udoovn.tar.bz2 -C /nfsroot`

## client (udoo neo)
1. Cấu hình cho board udoo neo:
- :pushpin: Gắn  cáp mạng, cáp serial
- :pushpin: Vào môi trường u-boot
- :computer: *uboot$* `env default -a -f` *reset biến môi trường về mặc định*
- :computer: *uboot$* `saveenv` *lưu lại biến môi trường*
- :computer: *uboot$* `setenv ipaddr 192.168.0.11` *set địa chỉ ip cho board udoo neo*
- :computer: *uboot$* `setenv serverip 192.168.0.10` *địa chỉ ip của ubuntu đã set ở trên* 
- :computer: *uboot$* `ping 192.168.0.10` *test thử ip ubuntu*
- :computer: *uboot$* `setenv nfsroot /nfsroot`  
- :computer: *uboot$* `setenv image zImage-udoovn.bin`  
- :computer: *uboot$* `setenv fdt_file zImage-udoovn.dtb`  
- :computer: *uboot$* `setenv ip_dyn no` *nếu dùng dhcp (ip động) thì bỏ qua bước này*
- :computer: *uboot$* `setenv netargs 'setenv bootargs console=${console},${baudrate} root=/dev/nfs ip=${ipaddr} rw nfsroot=${serverip}:${nfsroot},v3,tcp'`
- :computer: *uboot$* `saveenv`
- :computer: *uboot$* `run netboot`
2. Ghi chú:
- :pushpin: Do u-boot của mỗi board khác nhau nên việc cấu hình mỗi board có thể khác nhau. Ví dụ khi boot cho board *beaglebone hay pi* có thể  không giống ở trên
- :pushpin: thêm một số command của u-boot :
    ```
    help: hiện ra các lệnh trong u-boot
    printenv: in ra tất cả các biến môi trường
    print <cmd>: in ra giá trị của một biến nào đó
    
    ```
