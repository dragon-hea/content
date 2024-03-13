# Fixing KASLR

## KASLR là gì?

{% hint style="info" %}
Nó là viết tắt của cụm từ **`Kernel address space layout randomization`**. Nó là 1 công cụ bảo mật của Apple làm cho những Hacker khó có thể tìm ra những object quan trọng trong bộ nhớ vì nó luôn thay đổi ngẫu nhiên sau mỗi lần khởi động chi tiết xem [tại đây](https://lwn.net/Articles/569635/).
{% endhint %}

{% hint style="info" %}
Đối với những máy có memory maps nhỏ hoặc hoặc có quá nhiều device trong DSDT. Có thể sẽ có space cho kernel hoạt động nhưng cũng có thể sẽ có free space mà kernel không bao phủ tới. Vì thế ta sẽ không để macOS chọn 1 khu vực ngẫu nhiên để hoạt động trong mỗi lần reboot mà ta sẽ giới hạn khu vực bất kì mà chắc chắn nó sẽ hoạt động.
{% endhint %}

## Vậy khi nào cần Fix KASLR

> Thường khi boot macOS sẽ gặp các lỗi như sau:

```
Error allocating 0x1197b pages at 0x0000000017a80000 alloc type 2
Couldn't allocate runtime area

//hoặc

Only 244/256 slide values are usable!

//hoặc

panic(cpu 6 caller 0xffffff801fc057ba): a freed zone element has been modified in zone kalloc.4096: expected 0x3f00116dbe8a46f6 but found 0x3f00116d00000000
```

## Chuẩn bị:

B1: Tải OpenRuntime.efi&#x20;

> trong `OpenCorePKG` xem chi tiết [tại đây](../build-efi/creat-efi.md)

B2: Tải OpenShell.efi&#x20;

> xem cách tải `OpenShell` [tại đây](../build-efi/creat-efi.md)

B3: chỉnh `config --> Booter` theo sau:

* `AvoidRuntimeDefrag: YES`
* `DevirtualiseMmio: YES`
* `EnableSafeModeSlide: YES`
* `ProtectUefiServices: NO`
* `ProvideCustomSlide: YES`
* `RebuildAppleMemoryMap: YES`

## Setting BIOS

B1: Update BIOS&#x20;

> khá quan trọng đặc biệt là Z390

B2: Clear CMOS.

B3: Bật các option sau trong BIOS:

* Above4GDecoding&#x20;
  * đảm bảo rằng `Booter -> Quirks -> ResizeAppleGpuBars: 0`
* `Boot Options -> Windows8.1/10 mode`.
* Disable các option không cần thiết trong bios&#x20;
  * không cần thiết ở đây có nghĩa là bạn không sử dụng
    * CSM
    * Intel SGX
    * Parallel Port
    * Serial Port
    * iGPU
    * Thunderbolt
    * LED lighting
    * Legacy USB

## Tìm Slide Value

B1: Bạn chọn `OpenShell.efi` tại picker của OpenCore.

B2: Ấn Enter hoặc đợi vài giây.

B3: Các bạn gõ lệnh:

```
//truy cập vào ổ đĩa

fs0:

// kiểm tra xem bạn có đang ở đúng đường dẫn mong muốn không nếu không có thể thử fs1:

ls

// tạo file memmap.txt

memmap > memmap.txt
```

![](https://i.imgur.com/uQQPkHc.jpg)

B4: Mở file `memmap.txt` ra ta sẽ được:

```
Type       Start            End              # Pages          Attributes
BS_Code    0000000000000000-0000000000007FFF 0000000000000008 000000000000000F
Available  0000000000008000-000000000005DFFF 0000000000000056 000000000000000F
BS_Data    000000000005E000-000000000005FFFF 0000000000000002 000000000000000F
BS_Code    0000000000060000-000000000009EFFF 000000000000003F 000000000000000F
RT_Data    000000000009F000-000000000009FFFF 0000000000000001 800000000000000F
Available  0000000000100000-0000000000FFFFFF 0000000000000F00 000000000000000F
LoaderData 0000000001000000-00000000010FFFFF 0000000000000100 000000000000000F
Available  0000000001100000-000000001FFFFFFF 000000000001EF00 000000000000000F
Reserved   0000000020000000-00000000201FFFFF 0000000000000200 000000000000000F
Available  0000000020200000-0000000040003FFF 000000000001FE04 000000000000000F
Reserved   0000000040004000-0000000040004FFF 0000000000000001 000000000000000F
Available  0000000040005000-00000000CD431FFF 000000000008D42D 000000000000000F
LoaderCode 00000000CD432000-00000000CD53AFFF 0000000000000109 000000000000000F
BS_Data    00000000CD53B000-00000000CE025FFF 0000000000000AEB 000000000000000F
Available  00000000CE026000-00000000CE220FFF 00000000000001FB 000000000000000F
BS_Data    00000000CE221000-00000000CE5C2FFF 00000000000003A2 000000000000000F
Available  00000000CE5C3000-00000000CE5D6FFF 0000000000000014 000000000000000F
BS_Data    00000000CE5D7000-00000000CE762FFF 000000000000018C 000000000000000F
LoaderCode 00000000CE763000-00000000CE7EAFFF 0000000000000088 000000000000000F
BS_Data    00000000CE7EB000-00000000D90E3FFF 000000000000A8F9 000000000000000F
Available  00000000D90E4000-00000000D96C3FFF 00000000000005E0 000000000000000F
BS_Code    00000000D96C4000-00000000D9762FFF 000000000000009F 000000000000000F
RT_Code    00000000D9763000-00000000D9767FFF 0000000000000005 800000000000000F
BS_Code    00000000D9768000-00000000D97C5FFF 000000000000005E 000000000000000F
ACPI_NVS   00000000D97C6000-00000000D9DC6FFF 0000000000000601 000000000000000F
RT_Code    00000000D9DC7000-00000000D9DC9FFF 0000000000000003 800000000000000F
BS_Code    00000000D9DCA000-00000000D9DE0FFF 0000000000000017 000000000000000F
RT_Code    00000000D9DE1000-00000000D9DE6FFF 0000000000000006 800000000000000F
BS_Code    00000000D9DE7000-00000000D9DE8FFF 0000000000000002 000000000000000F
RT_Code    00000000D9DE9000-00000000D9DF6FFF 000000000000000E 800000000000000F
BS_Code    00000000D9DF7000-00000000D9F4DFFF 0000000000000157 000000000000000F
RT_Code    00000000D9F4E000-00000000D9F51FFF 0000000000000004 800000000000000F
BS_Code    00000000D9F52000-00000000D9F9CFFF 000000000000004B 000000000000000F
RT_Code    00000000D9F9D000-00000000D9FA3FFF 0000000000000007 800000000000000F
RT_Data    00000000D9FA4000-00000000D9FB0FFF 000000000000000D 800000000000000F
RT_Code    00000000D9FB1000-00000000D9FC2FFF 0000000000000012 800000000000000F
BS_Code    00000000D9FC3000-00000000D9FC5FFF 0000000000000003 000000000000000F
RT_Code    00000000D9FC6000-00000000D9FC7FFF 0000000000000002 800000000000000F
BS_Code    00000000D9FC8000-00000000D9FDEFFF 0000000000000017 000000000000000F
RT_Code    00000000D9FDF000-00000000D9FE4FFF 0000000000000006 800000000000000F
BS_Code    00000000D9FE5000-00000000D9FECFFF 0000000000000008 000000000000000F
RT_Code    00000000D9FED000-00000000D9FEDFFF 0000000000000001 800000000000000F
BS_Code    00000000D9FEE000-00000000D9FFCFFF 000000000000000F 000000000000000F
RT_Code    00000000D9FFD000-00000000D9FFDFFF 0000000000000001 800000000000000F
BS_Code    00000000D9FFE000-00000000DA008FFF 000000000000000B 000000000000000F
RT_Code    00000000DA009000-00000000DA00DFFF 0000000000000005 800000000000000F
BS_Code    00000000DA00E000-00000000DA039FFF 000000000000002C 000000000000000F
RT_Code    00000000DA03A000-00000000DA03AFFF 0000000000000001 800000000000000F
BS_Code    00000000DA03B000-00000000DA04AFFF 0000000000000010 000000000000000F
RT_Code    00000000DA04B000-00000000DA070FFF 0000000000000026 800000000000000F
BS_Code    00000000DA071000-00000000DA083FFF 0000000000000013 000000000000000F
RT_Code    00000000DA084000-00000000DA084FFF 0000000000000001 800000000000000F
BS_Code    00000000DA085000-00000000DA085FFF 0000000000000001 000000000000000F
RT_Code    00000000DA086000-00000000DA087FFF 0000000000000002 800000000000000F
BS_Code    00000000DA088000-00000000DA088FFF 0000000000000001 000000000000000F
RT_Code    00000000DA089000-00000000DA08DFFF 0000000000000005 800000000000000F
BS_Code    00000000DA08E000-00000000DA0A2FFF 0000000000000015 000000000000000F
RT_Data    00000000DA0A3000-00000000DA102FFF 0000000000000060 800000000000000F
RT_Code    00000000DA103000-00000000DA11DFFF 000000000000001B 800000000000000F
RT_Data    00000000DA11E000-00000000DA120FFF 0000000000000003 800000000000000F
RT_Data    00000000DA121000-00000000DA127FFF 0000000000000007 800000000000000F
RT_Data    00000000DA128000-00000000DA128FFF 0000000000000001 800000000000000F
RT_Data    00000000DA129000-00000000DA147FFF 000000000000001F 800000000000000F
Reserved   00000000DA148000-00000000DA4ADFFF 0000000000000366 000000000000000F
Reserved   00000000DA4AE000-00000000DA647FFF 000000000000019A 000000000000000F
ACPI_NVS   00000000DA648000-00000000DA70DFFF 00000000000000C6 000000000000000F
ACPI_NVS   00000000DA70E000-00000000DA8C7FFF 00000000000001BA 000000000000000F
ACPI_Recl  00000000DA8C8000-00000000DA8CCFFF 0000000000000005 000000000000000F
BS_Data    00000000DA8CD000-00000000DA8CDFFF 0000000000000001 000000000000000F
ACPI_NVS   00000000DA8CE000-00000000DA910FFF 0000000000000043 000000000000000F
BS_Data    00000000DA911000-00000000DAA5CFFF 000000000000014C 000000000000000F
BS_Code    00000000DAA5D000-00000000DACF6FFF 000000000000029A 000000000000000F
BS_Data    00000000DACF7000-00000000DAD06FFF 0000000000000010 000000000000000F
BS_Code    00000000DAD07000-00000000DAD18FFF 0000000000000012 000000000000000F
BS_Data    00000000DAD19000-00000000DAD1FFFF 0000000000000007 000000000000000F
RT_Data    00000000DAD20000-00000000DAFF3FFF 00000000000002D4 800000000000000F
BS_Data    00000000DAFF4000-00000000DAFFFFFF 000000000000000C 000000000000000F
Available  0000000100000000-000000011F1FFFFF 000000000001F200 000000000000000F
Reserved   00000000DBC00000-00000000DFDFFFFF 0000000000004200 8000000000000000
MMIO       00000000F8000000-00000000FBFFFFFF 0000000000004000 8000000000000001
MMIO       00000000FEC00000-00000000FEC00FFF 0000000000000001 8000000000000001
MMIO       00000000FED00000-00000000FED03FFF 0000000000000004 8000000000000001
MMIO       00000000FED1C000-00000000FED1FFFF 0000000000000004 8000000000000001
MMIO       00000000FEE00000-00000000FEE00FFF 0000000000000001 8000000000000001
MMIO       00000000FF000000-00000000FFFFFFFF 0000000000001000 8000000000000001

  Reserved  :         18,689 Pages (76,550,144 Bytes)
  LoaderCode:            401 Pages (1,642,496 Bytes)
  LoaderData:            256 Pages (1,048,576 Bytes)
  BS_Code   :          1,613 Pages (6,606,848 Bytes)
  BS_Data   :         47,748 Pages (195,575,808 Bytes)
  RT_Code   :            146 Pages (598,016 Bytes)
  RT_Data   :            876 Pages (3,588,096 Bytes)
  ACPI_Recl :              5 Pages (20,480 Bytes)
  ACPI_NVS  :          2,244 Pages (9,191,424 Bytes)
  MMIO      :         20,490 Pages (83,927,040 Bytes)
  MMIO_Port :              0 Pages (0 Bytes)
  PalCode   :              0 Pages (0 Bytes)
  Available :        969,334 Pages (3,970,392,064 Bytes)
  Persistent:              0 Pages (0 Bytes)
              -------------- 
Total Memory:          3,994 MB (4,188,663,808 Bytes)
```

B5: Bây giờ ta sẽ tiến hành chuyển giá trị thành `Slide value` chú ý vào giá trị `Available` lớn nhất ở cột `start`&#x20;

> chuyển sang decimal để so sánh

B6: Các bạn sẽ thực hiện tính toán theo công thức&#x20;

> chuyển sang decimal để tính toán

```
(HEX - 0x100000)/0x200000 = Slide Value in HEX

(DECIMAL - 0x100000)/0x200000 = Slide Value in decimal

(0000000100000000 - 0x100000)/0x200000

// convert hex to decimal 

(4294967296 - 1048576)/2097152 = 2047,5
```

![](https://i.imgur.com/LiarwbH.png)

B7: Nhưng con số này quá lớn (**> 256**) vì vậy ta cần phải hạ xuống các `Available` có giá trị nhỏ hơn cho đến khi bé hơn hoặc bằng 256.

B8: Ta sẽ thử với `0000000001100000` covert to decimal: `17825792` ta được kết quả là 8.

B9: Add vào boot-arg tham số `slide=XXX`&#x20;

> ở đây ta sẽ có là s`lide=8`

B10: Reboot.

<details>

<summary>Lưu ý</summary>

Ở 1 số máy sẽ nhận được slide value rất nhỏ như `slide=-0.379150390625` đối với trưởng hợp này ta sẽ đặt `slide=0`.

</details>

## Using DevirtualiseMmio

{% hint style="info" %}
Đối với 1 số main như Z390 và hầu hết dòng HEDT chúng ta cần lập 1 danh sách trắng. Đó là lúc MmioWhitelist phát huy tác dụng. Với **`DevirtualiseMmio`** và **`ProvideCustomSlide`** bạn vừa có thể bảo mật lại vừa có thể Boot, 1 combo hoàn hảo.
{% endhint %}

B1: Tạo 1 EFI Debug theo hướng dẫn [tại đây](https://heavietnam.ga/forum/topic/opencore-debugging/)&#x20;

> nhớ enable `DevirtualiseMmio`

* Đối với clover các bản cũng build tương tự (sử dụng bản clover debug) để dump được log file các bạn sẽ dùng `hackintool –> log`

B2: Bạn sẽ nhận được 1 file log mở nó ra và ta sẽ có:

```
21:495 00:009 OCABC: MMIO devirt start
21:499 00:003 OCABC: MMIO devirt 0x60000000 (0x10000 pages, 0x8000000000000001) skip 021:503 00:003 OCABC: MMIO devirt 0xFE000000 (0x11 pages, 0x8000000000000001) skip 021:506 00:003 OCABC: MMIO devirt 0xFEC00000 (0x1 pages, 0x8000000000000001) skip 021:510 00:003 OCABC: MMIO devirt 0xFED00000 (0x1 pages, 0x8000000000000001) skip 021:513 00:003 OCABC: MMIO devirt 0xFEE00000 (0x1 pages, 0x800000000000100D) skip 021:516 00:003 OCABC: MMIO devirt 0xFF000000 (0x1000 pages, 0x800000000000100D) skip 0
21:520 00:003 OCABC: MMIO devirt end, saved 278608 KB
```

Bây giờ ta có 6 regions cần thêm vào danh sách trắng (in đậm).

Ta cần convert hex to decimal (có thể dùng Hackintool).

* `MMIO devirt 0x60000000 -> 1610612736`
* `MMIO devirt 0xFE000000 -> 4261412864`
* `MMIO devirt 0xFEC00000 -> 4273995776`
* `MMIO devirt 0xFED00000 -> 4275044352`
* `MMIO devirt 0xFEE00000 -> 4276092928`
* `MMIO devirt 0xFF000000 -> 4278190080`

![](https://imgur.com/4a65sZl.png)

B3: Add vào `config ==> booter ==> MmioWhitelist.`

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/mmio-white.0ee7b5c7.png)

> **Source tham khảo:** [**Fixing KASLR slide values | OpenCore Install Guide (dortania.github.io)**](https://dortania.github.io/OpenCore-Install-Guide/extras/kaslr-fix.html)