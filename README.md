# 1. ES crack
[File](https://github.com/thune-work/Release_1/tree/main/File/ES%20crack): ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, with debug_info, not stripped

Sử dụng IDA pro 32-bit, tập trung vào đoạn code sau:

![IDApro](https://github.com/thune-work/Release_1/blob/main/Image/ES%20crack/IDApro.PNG)

Ta thấy rằng chỉ cần thêm vào chuỗi có các ký tự đầu bằng với password (0x35353450) thì khúc sau các ký tự có là gì cũng không quan trọng.

>FLAG: P445x (với x là chuỗi ký tự tự do)

![Result](https://github.com/thune-work/Release_1/blob/main/Image/ES%20crack/Result.PNG)

# 2.Lucky
[File](https://github.com/thune-work/Release_1/tree/main/File/Lucky):  ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, with debug_info, not stripped

File yêu cầu nhập vào 2 chữ số và chữ này lần lượt lưu tại byte_804A024 và byte_804A025.

![IDAPro](https://github.com/thune-work/Release_1/blob/main/Image/Lucky/IDAPro.PNG)

Trong đoạn code assembly trên, ta thấy byte_804A024 được chuyển từ ký tự thành số thông qua trừ cho 0x30 và lưu tại thanh ghi al. Còn byte_804A024 cũng được chuyển từ ký tự sang số và lưu tại thanh ghi bl. Sau đó, al = al + bl qua câu lệnh adc và được so sánh với 0x16. Mà bl + 0x30 = 0x38

Suy ra: bl = 8 => al = 8

>FLAG: 88

![Result](https://github.com/thune-work/Release_1/blob/main/Image/Lucky/Result.PNG)

# CrackMe_ASM
[File](https://github.com/thune-work/Release_1/tree/main/File/CrackMe_ASM): CrackMe_ASM: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, not stripped

Yêu cầu nhập vào giá trị cho biến var. Các byte của biến flag lần lược được gán giá trị rồi so sánh với chuỗi trong biến var vừa nhập. Nếu bằng nhau thì thành công mà không bằng nhau thì không thành công.

![IDA1](https://github.com/thune-work/Release_1/blob/main/Image/CrackMe_ASM/IDA1.PNG)
![IDA2](https://github.com/thune-work/Release_1/blob/main/Image/CrackMe_ASM/IDA2.PNG)

>FLAG: S3CrE+Fl4G!

![Result](https://github.com/thune-work/Release_1/blob/main/Image/CrackMe_ASM/Result.PNG)
