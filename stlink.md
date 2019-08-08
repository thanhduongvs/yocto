# Cài đặt stlink trên ubuntu

![Build Status](https://img.shields.io/badge/thanhduongvs-stlink%20linux-green.svg)

## Cài đặt:

01. **$** `sudo apt-get install make libtool pkg-config autoconf automake texinfo libusb-1.0-0-dev autotools-dev gtk+-3.0`  
02. **$** `git clone https://github.com/texane/stlink.git`  
03. **$** `cd stlink`  
04. *stlink* **$** `make -j8`  
05. *stlink* **$** `cd build/Release`  
06. *stlink/build/Release* **$** `sudo make install`

## Sử dụng:

01. Nạp file bin: `st-flash write path_file.bin 0x08000000`
02. Xem thông tin chip: `st-info --probe`
03. Xem hướng dẫn về stlink: `st-util --help`
04. Nếu chạy các lệnh trên gặp lỗi: **error while loading shared libraries: libstlink-shared.so.1** thì gõ lệnh `sudo ldconfig`

## Xem thêm:

https://github.com/texane/stlink
https://github.com/texane/stlink/blob/master/doc/compiling.md
https://github.com/texane/stlink/blob/master/doc/tutorial.md
