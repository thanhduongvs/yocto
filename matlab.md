#  Cài đặt matlab

1. Ngắt kết nối mạng
2. Cho 3 file MatlabR2018bLinux64Crack.zip, MatlabR2018b_LinuxX64_DVD1.iso, MatlabR2018b_LinuxX64_DVD2.iso vào cùng 1 thư mục
3. Cài gói 7zip để giải nén file iso `sudo apt-get install p7zip-full p7zip-rar`
4. Giải nén file Crack `unzip MatlabR2018bLinux64Crack.zip`
5. Giải nén DVD1 `7z x MatlabR2018b_LinuxX64_DVD1.iso`
5. Giải nén DVD2 `7z x MatlabR2018b_LinuxX64_DVD2.iso`
6. Sau khi giải nén sẽ có file **install**, thay đổi phân quyền cho file install `sudo chmod 777 install`

7. Chạy file install `sudo ./install`
8. Nếu gặp lỗi **./install: 1: exec: /home/thanhduong/Desktop/matlab/bin/glnxa64/install_unix: Permission denied** thì gõ lệnh `sudo chmod 777 -R ./`
9. Chạy lại lệnh `sudo ./install`
10. Chọn **Use a File Installation Key**
11. Nhập key **09806-07443-53955-64350-21751-41297**
12. Để ý Matlab sẽ được cài vào thư mục `/usr/local/MATLAB/R2018b`
13. Chờ cài đặt xong

14. Vào thư mục Crack `cd MatlabR2018bLinux64Crack`

15. Copy file *libmwlmgrimpl.so* trong thư mục crack vào thư mục cài đặt `sudo cp bin/glnxa64/matlab_startup_plugins/lmgrimpl/libmwlmgrimpl.so /usr/local/MATLAB/R2018b/bin/glnxa64/matlab_startup_plugins/lmgrimpl/`

16. Copy file license `sudo cp license_standalone.lic /usr/local/MATLAB/R2018b/license/`

17. Vào thư mục **license** `cd /usr/local/MATLAB/R2018b/license`
18. Gõ lệnh `ls` để xem có thấy file *license_standalone.lic* vừa copy không. Nếu không thì vẫn đứng ở thư mục vừa rồi copy lại `sudo cp /đường dẫn tới file crack/MatlabR2018bLinux64Crack/license_standalone.lic ./`
19. Trên terminal gõ lệnh `matlab` để chạy matlab. Nếu báo lỗi **command not found** thì làm thêm bước bên dưới
20. Mở file **.bashrc** bằng lệnh `gedit ~/.bashrc`. Xuống cuối file thêm dòng
`export PATH=$PATH:/usr/local/MATLAB/R2018b/bin`. Lưu lại xong và gõ lệnh
`source ~/.bashrc`. Gõ lại lệnh `matlab` để mở terminal
