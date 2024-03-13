# Cách xác định phần cứng

## CPU

{% hint style="info" %}
Ta dễ thấy đối với những guide hướng dẫn hackintosh hiện nay hầu hết đều dùng `Code Name` . Để xác định được `code name` của CPU và `iGPU` ta làm như sau:
{% endhint %}

B1: Bạn chuột phải vào biểu tượng `windows` ở góc dưới cùng bên trái và chọn `system`.

![](https://i.imgur.com/3QEza7m.png)

B2: Copy mục `processor`.

![](https://i.imgur.com/wwrPl8K.png)

B3: Ta tiến hành search mục `processor` trên trình duyệt.

![](https://i.imgur.com/wdh3pK5.png)

B4: Các bạn tiến hành mở kết quả tìm kiếm có domain là `ark.intel.com`

![](https://i.imgur.com/HeOXlJI.png)

B5: Bạn sẽ chú ý vào phần `tên mã` hoặc `code name`. Đây chính là `code name CPU` của bạn.

![](https://i.imgur.com/OADzcWa.png)

> Như ở đây ta có `code name` là `Comet Lake`

## IGPU

{% hint style="info" %}
Để có thể patch được `iGPU` thì ta phải xác định được `iGPU name.` Làm như sau:
{% endhint %}

B1: Ta sẽ tiến hành search `processor` trên trình duyệt và mở trang [sau](https://ark.intel.com/content/www/us/en/ark.html) như hướng dẫn ở `CPU`.

![](https://i.imgur.com/ngljFRK.png)

B2: Các bạn chú ý vào mục `Đồ họa Bộ xử lý --> Đồ họa bộ xử lý`

> Đây chính là `iGPU name` của bạn.

![](https://i.imgur.com/Y724Y6p.png)

> Ở đây `igpu name` là `HD4000`

## `Chipset model` và `Mainboard`

{% hint style="info" %}
Trước hết ta sẽ cần tìm hiểu chipset là gì? _Chipset máy tính là một_ mạch tích hợp giúp xử lý giao tiếp giữa CPU, RAM, bộ lưu trữ và các thiết bị ngoại vi khác. Chipset xác định số lượng thành phần tốc độ cao hoặc thiết bị USB mà bo mạch chủ của bạn có thể hỗ trợ. Chipset thường bao gồm một đến bốn chip và các bộ điều khiển tính năng cho các thiết bị ngoại vi thường được sử dụng, như bàn phím, chuột hoặc màn hình.



Vậy còn mainboard là gì? Mainboard hay main máy tính hay bo mạch chủ là một bảng mạch đóng vai trò nền tảng trên máy tính, laptop có tác dụng kết nối các linh kiện bên trong thành thể thống nhất. Mainboard PC sẽ nằm ở thùng máy, hoặc tích hợp đằng sau màn hình đối với máy tính AIO.



Để xác định được mã của chúng ta sẽ làm như sau
{% endhint %}

### Device manager

B1: mở `device manager`

B2: chọn tới tab `IDE ATA/ATAPI controllers`

B3: Chọn `properties` ta sẽ có thể thấy thế hệ `chipset`

![](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-11-49-06-299460458\_838685037052875\_6042626987910816592\_n.png)

> Hoặc truy cập `System devices` và xem chipset series ở đây
>
> ![](<../.gitbook/assets/image (130).png>)

### Aida64

B1: tải [Aida64](https://hackintosher-my.sharepoint.com/personal/hoanglongvonguyen\_hackintosher\_onmicrosoft\_com/\_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool%2FAIDA64%206%2E88%2E6400%20Final%20ReP4ck%2Erar\&parent=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool\&ga=1)

> Pass: heavietnam

B2: Truy cập `Motherboard --> Motherboard --> Motherboard Physical Info --> Motherboard Chipset --> Vaule`

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

## Audio Codec

B1: Các bạn tải `aida64` [tại đây](https://www.aida64.com/downloads).

> Hoặc [tại đây](https://hackintosher-my.sharepoint.com/personal/hoanglongvonguyen\_hackintosher\_onmicrosoft\_com/\_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool%2FAIDA64%206%2E88%2E6400%20Final%20ReP4ck%2Erar\&parent=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool\&ga=1)
>
> > Pass: heavietnam

B2: Các bạn sẽ vào tab `Multimedia --> PCI/PnP Audio` ở đây ta sẽ có thể thấy `audio codec`.

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/audio-controller-aida64.c4a94a0b.png)

## Network Controller models

B1: Các bạn tải `aida64` [tại đây](https://www.aida64.com/downloads).

> Hoặc [tại đây](https://hackintosher-my.sharepoint.com/personal/hoanglongvonguyen\_hackintosher\_onmicrosoft\_com/\_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool%2FAIDA64%206%2E88%2E6400%20Final%20ReP4ck%2Erar\&parent=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool\&ga=1)
>
> > Pass: heavietnam

B2: Các bạn sẽ chú ý đến mục sau `Network --> PCI/PnP Network`.

![](https://i.imgur.com/XiTPa8B.png)

## Drive Model

B1: Chuột phải vào `logo Windows` ở góc cuối bên trái và chọn `device manager`.

![](https://i.imgur.com/oNBcTmN.png)

B2: Bạn chọn vào `disk` ở đây ta sẽ có được tên ổ cứng.

![](https://i.imgur.com/vL2khbW.png)

## dGPU

B1: Bạn nhấn tổ hợp phím `Windows + R` và gõ `dxdiag` sau đó enter.

![](https://i.imgur.com/XHk4Jxy.png)

B2: Hệ thống sẽ mở `DirectX Diagnostic Tool` và bạn hãy chuyển đến tab `render` chú ý đến dòng name.

![](https://i.imgur.com/zzNSJad.png)

## `Keyboard`, `Trackpad` và `Touchscreen` `Connection Type`

### Aida64

B1: Các bạn tải aida64 [tại đây](https://www.aida64.com/downloads)

> Hoặc [tại đây](https://hackintosher-my.sharepoint.com/personal/hoanglongvonguyen\_hackintosher\_onmicrosoft\_com/\_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool%2FAIDA64%206%2E88%2E6400%20Final%20ReP4ck%2Erar\&parent=%2Fpersonal%2Fhoanglongvonguyen%5Fhackintosher%5Fonmicrosoft%5Fcom%2FDocuments%2Ftool\&ga=1)
>
> > Pass: heavietnam

B2: Các bạn truy cập vào đường dẫn `Devices --> Physical Devices --> PnP Devices` ở đây các bạn sẽ tiến hành tìm kiếm name `Touchpad`.

![](https://i.imgur.com/PgeqvQY.png)

### Device manager

B1: Nhấn chuột phải vào `logo windows --> Device manager --> Device by connection`

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

Bạn có thể xác định các device thông qua nhưng phần dưới đây

* `Human Interface Devices`
* `Keyboards`
* `Mice and other Pointer Devices`

> Thông qua những giá trị đó bạn có thể xác định được nó thuộc dạng `PS2, SMBUS, USB hay i2c`

<details>

<summary>SMBus</summary>

Nó sẽ hiển thị các dạng như `Synaptics SMBus Driver` hoặc`ELAN SMBus Driver`

* Synaptics sẽ có dạng như sau
  * Synaptics PS2 device
  * Synaptics Pointing Device
  * Synaptics SMBus Driver

<img src="../.gitbook/assets/image (17).png" alt="" data-size="original">

Như ở đây là một trường hợp Touchpad có 2 phương thức là PS2 và SMBUS. Đương nhiên bạn có thể sử dụng một trong 2 phương thức. Nhưng đối với SMBus thường sẽ cung cấp độ chính xác và hỗ trợ tốt hơn

</details>

<details>

<summary>USB</summary>

![](<../.gitbook/assets/image (48).png>)

Ở đây ta có thể thấy `touchpad devices` nằm ở dưới `usb controller`&#x20;

</details>

<details>

<summary>I2C</summary>

![](<../.gitbook/assets/image (121).png>)

Ở đây ta có thể thấy `Touchpad devices` nằm ở dưới `I2c controller`

</details>

## Xác định `location path`

{% hint style="info" %}
Trước tiên ta cần tìm hiểu `location path` là gì? Nó là đường dẫn đến `devices` trong hệ thống và thường được hệ thống đọc dưới 2 dạng là `ACPI path` và `Device path`. `ACPI path` thường được sử dụng trong việc `patch DSDT` còn `Device path` thường được dùng trong `config.plist` để inject `properties` của các `Device`.
{% endhint %}

B1: Các bạn nháy chuột phải vào biểu tượng `Windows` ở góc dưới bên trái và chọn `device manager`.

![](https://i.imgur.com/oNBcTmN.png)

B2: Tiến hành chuột phải vào device cần lấy `location path` và chọn `properties`.

![](https://i.imgur.com/2bzgPTP.png)

B3: Chọn tab `Details --> location paths`.

![](https://i.imgur.com/strrnkL.png)

B4: Ta sẽ tiến hành chuyển value vừa lấy được ở trên về dạng path để có thể sử dụng.

```
//value mặc định
PCIROOT(0)#PCI(1C01)#PCI(0000)
ACPI(_SB_)#ACPI(PCI0)#ACPI(RP02)#ACPI(WLAN)

//Device path mặc định
PCIROOT(0)#PCI(1C01)#PCI(0000)

//ACPI path mặc định 
ACPI(_SB_)#ACPI(PCI0)#ACPI(RP02)#ACPI(WLAN)

//Convert Device path mặc định
PCIROOT(0)#PCI(1C01)#PCI(0000)
# Tiến hành chuyển dấu "#" thành dấu "/"
PCIROOT(0)/PCI(1C01)/PCI(0000)
# Ta tiến hành chia các giá trị nhỏ trong "()" thành các cặp 2 số. Nếu cặp số không bắt đầu bằng số "0" thì thêm số "0" vào trước
PCIROOT(00)/PCI(01C 01)/PCI(00 00)
# Viết thế chữ "x" vào giữa các cặp 2 chữ số
PCIROOT(0x0)/PCI(0x1C 0x1)/PCI(0x0 0x0)
# chuyển các dấu cách trong ngoặc thành dấu ","
PCIROOT(0x0)/PCI(0x1C,0x1)/PCI(0x0,0x0)

// Convert ACPI path nguyên mẫu
# Ta tiến hành giữ lại các ký tự trong ngoặc những ký tự khác thì lược bỏ 
_SB_ PCI0 RP02 WLAN
# Tiếp đó tiến hành thay dấu cách bằng dấu "."
_SB_.PCI0.RP02.WLAN
```

## Xác định `vendor id` và `device id` của `usb controller`

{% hint style="info" %}
Khi tiến hành map usb các bạn cần xác định device id và vendor id để sử dụng đúng kext. Việc này rất dễ thực hiện ở mac nhưng khi không có mac thì các bạn làm theo sau:
{% endhint %}

B1: Chuột phải vào biểu tượng windows chọn `device manager`

B2: Chuột phải vào tên usb controller và chọn `Properties`

> Tên usb controller thường bắt đầu bằng `Intel (R)`

![](https://i.imgur.com/8l5Fdg8.png)

B3: Tới tab `Details` và chọn `Hardware ID` và nhìn vào dòng đầu tiên

![](https://i.imgur.com/pu11YG6.png)

> Ở đây ta có vendor id là `8086` device id là `1E31`

> Source tham khảo: [Finding your hardware | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/find-hardware.html#finding-hardware-using-windows)
