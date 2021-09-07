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

# 3. CrackMe_ASM
[File](https://github.com/thune-work/Release_1/tree/main/File/CrackMe_ASM): CrackMe_ASM: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, not stripped

Yêu cầu nhập vào giá trị cho biến var. Các byte của biến flag lần lược được gán giá trị rồi so sánh với chuỗi trong biến var vừa nhập. Nếu bằng nhau thì thành công mà không bằng nhau thì không thành công.

![IDA1](https://github.com/thune-work/Release_1/blob/main/Image/CrackMe_ASM/IDA1.PNG)
![IDA2](https://github.com/thune-work/Release_1/blob/main/Image/CrackMe_ASM/IDA2.PNG)

>FLAG: S3CrE+Fl4G!

![Result](https://github.com/thune-work/Release_1/blob/main/Image/CrackMe_ASM/Result.PNG)

# 4. hello
[File](https://github.com/thune-work/Release_1/tree/main/File/hello): ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped

![IDA1](https://github.com/thune-work/Release_1/blob/main/Image/hello/IDA1.PNG)

Đầu tiên, chúng ta nhập chuỗi username, sau đó chuỗi welcome bằng "Hello " + username. Password nhập vào lưu trong buf.

![IDA3](https://github.com/thune-work/Release_1/blob/main/Image/hello/IDA3.PNG)

Ở đây, chúng ta thấy welcome[v4 + 5] + 5 == byte_402073[v4]. Chúng ta xem thử byte_402073 rốt cuộc là cái qq gì.

![IDA2](https://github.com/thune-work/Release_1/blob/main/Image/hello/IDA2.PNG)

Nếu như byte_402073 ở vị trí 0x402073 thì chuỗi buf đang lưu password ở vị trí 0x402074. Suy ra v4 = 1 sẽ là byte đầu tiên của chuỗi buf. Và password ở đây cũng chỉ có 1 ký tự dựa vào dòng lệnh if (!--v4).

Vậy welcome[6] + 5 = buf[0] => ký tự đầu tiên của username + 5 bằng password. Nhập username là thune => password: y

>USERNAME: thune FLAG: y

![Result](https://github.com/thune-work/Release_1/blob/main/Image/hello/Result.PNG)

# 5. nasm
[File](https://github.com/thune-work/Release_1/tree/main/File/nasm): ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped

![IDA1](https://github.com/thune-work/Release_1/blob/main/Image/nasm/IDA1.PNG)

Flag được nhập vào sẽ được lưu tại địa chỉ 0x402031 sau đó đem đi so sánh 0xB ký tự đầu với chuỗi passwd. Chuỗi passwd bao gồm các ký tự sau:

![IDA2](https://github.com/thune-work/Release_1/blob/main/Image/nasm/IDA2.PNG)

passwd = supersecret

> FLAG: supersecretx, với x là 1 chuỗi ngẫu nhiên

![Result](https://github.com/thune-work/Release_1/blob/main/Image/nasm/Result.PNG)

# 6. Clone
[File](https://github.com/thune-work/Release_1/tree/main/File/clone): PE32 executable (GUI) Intel 80386, for MS Window

![image](https://github.com/thune-work/Release_1/blob/main/Image/clone/image.PNG)

Đầu tiên, chúng ta kiểm tra thử list strings của bài này:

![strings](https://github.com/thune-work/Release_1/blob/main/Image/clone/strings.PNG)

Chúng ta để ý có các chuỗi "Well done! Now make good tutorial :)", "Bravo!" có thể là các chuỗi thông báo đã nhập đúng User và Serial.

Tiếp theo chúng ta xem thử các chuỗi này nằm ở hàm nào và hàm đó liên quan tới những hàm nào.

![graph.PNG](https://github.com/thune-work/Release_1/blob/main/Image/clone/graph.PNG)

Hàm start là hàm khởi đầu gọi hàm sub_40101D để load các control (textbox, label, button) cho window form. Các control có số hiệu nhận dạng là 101, 102, 104 được lưu ở các biến v4, v2, v1. Sau đó, hàm sub_40101D gọi hàm sub_401180 để làm hành động cho các control này.

![sub_40101D](https://github.com/thune-work/Release_1/blob/main/Image/clone/sub_40101D.PNG)

Để pass, chúng ta cần xem xét kỹ hàm sub_401180.

![sub_401180(1)](https://github.com/thune-work/Release_1/blob/main/Image/clone/sub_401180(1).PNG)

Câu lệnh này chứa hàm GetDlgItemTextA() là hàm cho đầu ra là số lượng ký tự của string nhập vào, từ đó chúng ta suy ra trong 2 control mang số nhận dạng 101 hoặc 102, 1 cái là User nhập vào, 1 cái là Serial nhập vào. Nếu là control 101 thì string được lưu ở byte_40307C có số ký tự phải lớn hơn 5, còn 102 thì lưu ở String có số ký tự bằng 8.

Để xác định đâu là User, đâu là Serial, ta đặt break point trong hàm sub_401180 tại câu lệnh:

![breakpoint1](https://github.com/thune-work/Release_1/blob/main/Image/clone/breakpoint1.PNG)

Sau đó debug, nhập "11111" vào User, "22222" vào Serial và xem kết quả tại biến byte_40307C và String, ta được kết quả như sau:

![debug1](https://github.com/thune-work/Release_1/blob/main/Image/clone/debug1.PNG)

Như vậy, Serial được lưu trong String, User được lưu trong biến byte_40307C

Đoạn code từ dòng 29-57 không làm thay đổi giá trị byte_40307C và cũng không so sánh điều kiện để thoả chương trình nên chúng ta không cần quan tâm đến nó. Chúng ta kiểm tra đoạn code chứa dòng lệnh hiện messageBox thông báo bypass chương trình như sau.

![sub_401180(2)](https://github.com/thune-work/Release_1/blob/main/Image/clone/sub_401180(2).PNG)

Như vậy, có 2 vấn đề chúng ta quan tâm ở đây là giá trị của biến dword_4030C8 và giá trị biểu thức bên trái dấu bằng có liên quan hàng loạt đến mảng byte_4030B8. Hai giá trị này phải bằng. Đầu tiên, giá trị của biến dword_4030C8 có liên quan đến byte_40307C (User) qua một nùi code sau:

![User](https://github.com/thune-work/Release_1/blob/main/Image/clone/User.PNG)

dword_4030C8 được tính toán dựa trên User và ở đây không có kiểm tra bất kỳ điều kiện thêm nào giành cho User. Do đó, với một User bất kỳ nhập vào, ta luôn tính được giá trị của biến này tương ứng.

Còn mảng byte_4030B8 liên quan đến đoạn code sau:

![Serial](https://github.com/thune-work/Release_1/blob/main/Image/clone/Serial.PNG)

Đoạn code này có chức năng chuyển các ký tự trong chuỗi String thành các số. Nếu là ký tự số như "1" thì được trừ cho 48 chuyển thành số 0x1. Nếu là kí tự in hoa từ vị trí 0x41 đến 0x46 trong bảng mã ASCII, ví dụ như "A" thì được trừ cho 55 (65-55 = 10 = 0xa). Các trường hợp còn lại không phải ký tự thuộc mã hex thì không hợp lệ. Tóm lại, nếu nhập chuỗi "aabbcc" thì mảng byte_4030B8 sẽ gồm {0xa, 0xa, 0xb, 0xb, 0xc, 0xc}. Từ đó, ta biết được chuỗi nhập vào String phải gồm 8 ký tự, các ký tự in hoa và là một mã hex.

Điều kiện phải được thoả trong câu lệnh if như sau:

dword_4030C8 = _ byteswap_ulong((((byte_4030B8[7] + 16 * byte_4030B8[6]) ^ 0xCD) - 17) 
                              + (((((byte_4030B8[5] + 16 * byte_4030B8[4]) ^ 0x90) - 85)
                               + (((((byte_4030B8[3] + 16 * byte_4030B8[2]) ^ 0x56) + 120)
                                 + ((((byte_4030B8[1] + 16 * byte_4030B8[0]) ^ 0x12) + 52) << 8)) << 8)) << 8)))
                                 
Do mỗi phần tử của mảng byte_4030B8 có 4 bits nên khi lấy byte_4030B8[i] * 16 + byte_4030B8[i+1] sẽ thành một byte. _ byteswap_ulong là hàm có đầu ra là một số viết theo little-endian. Tóm lại là như sau:

s[0] ^ 0x12 + 52 = dword_4030C8[3]
s[1] ^ 0x56 + 120 = dword_4030C8[2]
s[2] ^ 0x90 - 85 = dword_4030C8[1]
s[3] ^ 0xCD - 17 = dword_4030C8[0]

Với s[i] là byte do 4 bit của phần tử byte_4030B8[i] và phần tử byte_4030B8[i+1] tạo thành.

Để tìm được dword_4030C8, ta kiểm tra xem biến này được tính giá trị đặt trong thanh ghi nào rồi đặt breakpoint tại thời điểm giá trị được tính ra để nhìn thấy kết quả. Lưu ý phải nhập Serial với chuỗi bất kỳ 8 ký tự để có thể bypass điều kiện, User chọn ngẫu nhiên ở đây là "thune1".

![value1.PNG](https://github.com/thune-work/Release_1/blob/main/Image/clone/value1.PNG)

Ta thấy rằng giá trị của dword_4030C8 được lưu ở thanh ghi ebx nên ta debug và kiểm tra giá trị ebx.

![Value2.PNG](https://github.com/thune-work/Release_1/blob/main/Image/clone/value2.PNG)

s[0] ^ 0x12 + 0x34 = 0x0f
s[1] ^ 0x56 + 0x78 = 0xc6
s[2] ^ 0x90 + 0xab = 0x5c
s[3] ^ 0xcd + 0xef = 0x8b

Dựa vào mã assembly của các phép tính này để suy ngược lại ra s[i].

![value3](https://github.com/thune-work/Release_1/blob/main/Image/clone/value3.PNG)

1 byte s[0] sẽ được lưu trong ebx, sau đó ta chỉ lấy thanh ghi bl (8 bits của thanh ghi ebx) xor với 0x12, rồi cộng với 0x34. Lúc này giá trị có thể quá lớn và vượt qua 8 bit nên and với 0xff để lấy chỉ 8 bit. Ta suy ngược lại cách tính s[0] như sau: (0x0f - 0x34)^0x12. Nhưng vì các phép tính + hoặc trừ có thể sinh ra số âm nên ta phải cộng thêm với 256, đồng thời do chỉ cộng thanh ghi bl với 0x34, tức chỉ lấy 8 bit cộng với 0x34, nên khi suy ngược lại phải thêm and 0xff để lấy chỉ 8 bit. 

Cuối cùng,
s[0] = ((0x0f - 0x34 + 256)&0xff)^0x12
s[1] = ((0xc6 - 0x78 + 256)&0xff)^0x56
s[2] = ((0x5c - 0xab + 256)&0xff)^0x90
s[3] = ((0x8b - 0xef + 256)&0xff)^0xcd

> Ta được Serial = C9182151 với User = "thune1"

![done.PNG](https://github.com/thune-work/Release_1/blob/main/Image/clone/done.PNG)
