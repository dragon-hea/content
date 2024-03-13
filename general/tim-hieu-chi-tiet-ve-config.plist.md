# Tìm hiểu chi tiết về config.plist

<details>

<summary>Các điều cơ bản cần biết</summary>

* Các key `#WARNING - x` có thể xoá nếu muốn, không có ảnh hưởng gì.

<!---->

* Một config.plist sẽ luôn bao gồm các phần và cần chỉnh sửa như sau:
  * ACPI: Can thiệp ACPI bằng việc bổ sung và vá nhằm tăng tính tương thích.
  * Booter: Gồm các bản vá cần thiết cho hệ thống UEFI
    * Đa số áp dụng cho trình khởi động Apple.
  * Device Properties: Thêm một số thuộc tính cần thiết cho các thiết bị PCI.
  * Kernel: Nhằm can thiệp đến `Apple Kernel (XNU)` bằng việc thêm kext
    * Vá kernel và chặn kext hoạt động.&#x20;
  * Misc: Chức các tuỳ chọn về khởi động của OpenCore
    * Tuân theo chính sách khởi động của Apple.
  * NVRAM: Cung cấp thêm các biến trong NVRAM cần thiết cho macOS.
  * Platforminfo: Bao gồm các thông tin máy Mac được tạo ra dựa theo AppleModels nhằm tương thích với các dịch vụ của macOS
  * UEFI: Tại đây cho phép tải thêm một số UEFI module cùng các bản vá cần thiết.

</details>

{% hint style="info" %}
Hãy nhớ rằng mọi thay đổi về&#x20;

* ACPI
* Kexts
* Drivers
* Tool

Trong EFI đều cần tiến hành `OC_Snapshot`
{% endhint %}

<details>

<summary>Cách OC_Snapshot</summary>

B1: Mở Config.plist với [Propertree](https://github.com/corpnewt/ProperTree)

B2: Chọn `File --> OC_Snapshot`

![](<../.gitbook/assets/image (3).png>)

B3: Chọn vào folder `OC` và chọn `Select folder`

![](<../.gitbook/assets/image (4).png>)

B4: Ấn `Save` lại và vậy là xong

</details>

## ACPI

### Add

{% hint style="info" %}
Đây là nơi inject các SSDT và DSDT trong `config.plist`

> Các bạn chỉ việc `OC_Snapshot` là được không cần làm gì thêm
{% endhint %}

### Delete

{% hint style="info" %}
* Nhằm chặn hoạt động của một hay nhiều bảng ACPI có sẵn.
* Với CPU Intel Sandy Bridge và Ivy Bridge&#x20;
  * Không áp dụng với dòng HEDT
  * Điều này là cần thiết bởi XCPM của Apple có thể gây lỗi `AppleIntelCPUPowerManagement`, hãy đặt giá trị `True` với 2 key là `All` và `Enabled`.
{% endhint %}

Dưới đây là bảng thông tin và công dụng của các dòng trong `Delete`

<table><thead><tr><th width="166" align="center">Key</th><th width="171.33333333333331" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">All</td><td align="center">Boolean</td><td>Nhằm chặn tất cả ACPI phù hợp</td></tr><tr><td align="center">Comment</td><td align="center">String</td><td>Giải thích tác dụng, để trống hoặc ghi thêm tuỳ ý</td></tr><tr><td align="center">Enabled</td><td align="center">Boolean</td><td>Cho phép áp dụng hoặc không</td></tr><tr><td align="center">OemTableId</td><td align="center">Data</td><td>Xác định bảng ACPI bằng OEM ID</td></tr><tr><td align="center">TableLength</td><td align="center">Number</td><td>Giá trị mặc định phù hợp với mọi bảng ACPI</td></tr><tr><td align="center">TableSignature</td><td align="center">Data</td><td>Áp dụng với loại bảng ACPI, phổ biến là <code>SSDT</code></td></tr></tbody></table>

### Patch

{% hint style="info" %}
Đây là nơi add các patch rename nhằm rename các`ACPI-ID`trong DSDT mà không cần sửa trực tiếp

> Xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/acpi-advance/patch-dsdt-phan-3)
{% endhint %}

Dưới đây là bảng thông tin và công dụng của các dòng trong `Patch`

<table><thead><tr><th width="168.33333333333331" align="center">Key</th><th width="143" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Base</td><td align="center">String</td><td>Áp dụng với một hoặc bất kì thiết bị PCI phù hợp</td></tr><tr><td align="center">BaseSkip</td><td align="center">Number</td><td>Số lần áp dụng với <code>Base</code> khi được tìm thấy. Giá trị 0 tức không giới hạn</td></tr><tr><td align="center">Comment</td><td align="center">String</td><td>Giải thích tác dụng, để trống hoặc ghi thêm tuỳ ý</td></tr><tr><td align="center">Count</td><td align="center">Number</td><td>Số lần áp dụng khi được tìm thấy. Giá trị 0 tức không giới hạn</td></tr><tr><td align="center">Enabled</td><td align="center">Boolean</td><td>Cho phép áp dụng hoặc không</td></tr><tr><td align="center">Find</td><td align="center">Data</td><td>Giá trị cần tìm ở dạng HEX</td></tr><tr><td align="center">Limit</td><td align="center">Number</td><td>Số lần tối đa được tìm kiếm. Giá 0 tức không giới hạn</td></tr><tr><td align="center">Mask</td><td align="center">Data</td><td>Giá trị bitwise mask dành cho <code>Find</code>. Nếu sử dụng cần áp dụng tương ứng với <code>Find</code></td></tr><tr><td align="center">OemTableId</td><td align="center">Data</td><td>Xác định bảng ACPI bằng OEM ID. Để trống tức thay đổi cho tất cả</td></tr><tr><td align="center">Replace</td><td align="center">Data</td><td>Giá trị thay thế ở dạng HEX</td></tr><tr><td align="center">ReplaceMask</td><td align="center">Data</td><td>Giá trị bitwise mask dành cho <code>Replace</code>. Nếu sử dụng cần áp dụng tương ứng với <code>Replace</code></td></tr><tr><td align="center">Skip</td><td align="center">Number</td><td>Số lần xuất hiện cần bỏ qua trước khi áp dụng. Giá trị 0 tức không bỏ qua lần nào</td></tr><tr><td align="center">TableLength</td><td align="center">Number</td><td>Giá trị mặc định phù hợp với mọi bảng ACPI</td></tr><tr><td align="center">TableSignature</td><td align="center">Data</td><td>Áp dụng với loại bảng ACPI. Để trống tức thay đổi cho tất cả</td></tr></tbody></table>

### Quirks

<table><thead><tr><th width="189.33333333333331" align="center">Key</th><th width="134" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">FadtEnableReset</td><td align="center">Boolean</td><td>Sửa đổi trong bảng FADT giúp shutdown/reset hoạt động. Chủ yếu dành cho một số phần cứng cũ và một số laptop mới khi gặp lỗi</td></tr><tr><td align="center">NormalizeHeaders</td><td align="center">Boolean</td><td>Chỉ dành cho macOS High Sierra 10.13 khi gặp <code>com.apple.driver.AppleACPIPlatform</code> panic</td></tr><tr><td align="center">RebaseRegions</td><td align="center">Boolean</td><td>Xác định lại bộ nhớ ACPI, không được khuyến khích</td></tr><tr><td align="center">ResetHwSig</td><td align="center">Boolean</td><td>Đặt giá trị <code>HardwareSignature</code> thành 0 trong bảng FACS</td></tr><tr><td align="center">ResetLogoStatus</td><td align="center">Boolean</td><td>Đặt giá trị <code>Displayed status</code> thành False trong bảng BGRT</td></tr><tr><td align="center">SyncTableIds</td><td align="center">Boolean</td><td>Đồng bộ hoá các bảng ACPI đã được vá không tương thích với bảng SLIC ảnh hướng đến Windows phiên bản cũ</td></tr><tr><td align="center">EnableForAll</td><td align="center">Boolean</td><td><strong>Chỉ sử dụng với OpenCore_NO_ACPI, hãy xoá đi (nếu có) khi dùng phiên bản OpenCore gốc</strong><br>Cho phép áp dụng các bản vá ACPIs với tất cả hệ điều hành khi được khởi động qua hoặc không</td></tr></tbody></table>

## Booter

### MmioWhitelist

{% hint style="info" %}
Yêu cầu quirk `DevirtualiseMmio` được bật để hoạt động.&#x20;

> Xem chi tiết [tại đây](../issue/fixing-kaslr.md)&#x20;
{% endhint %}

<table><thead><tr><th width="137.33333333333331" align="center">Key</th><th width="132" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Address</td><td align="center">Number</td><td>Địa chỉ MMIO cần được chặn</td></tr><tr><td align="center">Comment</td><td align="center">String</td><td>Giải thích tác dụng, để trống hoặc ghi thêm tuỳ ý</td></tr><tr><td align="center">Enabled</td><td align="center">Boolean</td><td>Cho phép áp dụng hoặc không</td></tr></tbody></table>

### Patch

{% hint style="info" %}
Tôi không có kiến thức về phần này nhưng dường như nó cũng không quan trọng bạn có thể bỏ qua nó
{% endhint %}

### Quirks

<table><thead><tr><th width="243" align="center">Key</th><th width="155.33333333333331" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">AvoidRuntimeDefrag</td><td align="center">Boolean</td><td>Bắt buộc với mọi phần cứng nhằm sửa lỗi ngày giờ, NVRAM, điều khiển nguồn,…</td></tr><tr><td align="center">DevirtualiseMmio</td><td align="center">Boolean</td><td>Tìm hiểu thêm tại hướng dẫn MmioWhitelist</td></tr><tr><td align="center">EnableSafeModeSlide</td><td align="center">Boolean</td><td>Cần thiết khi Safe Mode ở macOS không hoạt động. Yêu cầu mainboard hỗ trợ UEFI</td></tr><tr><td align="center">EnableWriteUnprotector</td><td align="center">Boolean</td><td>Cho phép chỉnh sửa UEFI Runtime Services(CR0)</td></tr><tr><td align="center">ProtectMemoryRegions</td><td align="center">Boolean</td><td>Chỉ dành cho laptop Chromebook dùng UEFI coreboot gặp lỗi shutdown/restart</td></tr><tr><td align="center">ProtectUefiServices</td><td align="center">Boolean</td><td>Chặn việc ghi đè cho UEFI services dành cho CPU Intel Ice Lake và motherboard Z390</td></tr><tr><td align="center">ProvideCustomSlide</td><td align="center">Boolean</td><td>Sửa lỗi <code>OCABC: Only N/256 slide values are usable!</code>, có thể tắt nếu không cần thiết</td></tr><tr><td align="center">RebuildAppleMemoryMap</td><td align="center">Boolean</td><td>Dành cho phần cứng có bảng MAT, tạo Memory Map tương thích cho macOS</td></tr><tr><td align="center">ResizeAppleGpuBars</td><td align="center">Number</td><td>Nếu Resizable BAR Support được hỗ trợ và bật trong BIOS hãy để giá trị là <code>0</code>, còn không hãy để nguyên <code>-1</code></td></tr><tr><td align="center">SetupVirtualMap</td><td align="center">Boolean</td><td>Sửa lỗi của SetVirtualAddresses. Những motherboard Gigabyte có thể cần bật nếu gặp kernel panic. Với những mainboard ASUS, Gigabyte, ASRock Z490 sẽ không cần lựa chọn này</td></tr><tr><td align="center">SyncRuntimePermissions</td><td align="center">Boolean</td><td>Sửa lỗi bảng MAT và buộc macOS, Windows, Linux,… khởi động bằng bảng MAT</td></tr><tr><td align="center">EnableForAll</td><td align="center">Boolean</td><td><strong>Chỉ sử dụng với OpenCore_NO_ACPI, hãy xoá đi (nếu có) khi dùng phiên bản OpenCore gốc</strong><br>Cho phép áp dụng Booter với tất cả hệ điều hành khi được khởi động qua hoặc không</td></tr></tbody></table>

## DeviceProperties

### Add

{% hint style="info" %}
Như tên của nó đây là phần giúp các bạn thêm các thuộc tính hay gọi là properties vào các device PCIe
{% endhint %}

{% hint style="info" %}
Một số guide bạn có thểm tham khảo

* [Patch IGPU](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/patch-igpu)
* [Patch Bus-ID](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/fix-connector)
* [Disable NVME](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/acpi-advance/disable-unsupported-nvme)
* [Disable DGPU](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/disable-dgpu-desktop)
{% endhint %}

### Delete

{% hint style="info" %}
Tôi không có kiến thức về phần này&#x20;

> Nhưng nó cũng không có gì quan trọng bạn có thể bỏ qua
{% endhint %}

## Kernel

### Add

{% hint style="info" %}
Đây là nơi inject các Kext trong `config.plist`

> Các bạn chỉ việc `OC_Snapshot` là được không cần làm gì thêm\
>
{% endhint %}

Dưới đây là công dụng của từng dòng inject kext trong kernel

<table><thead><tr><th width="173.33333333333331" align="center">Key</th><th width="133" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Arch</td><td align="center">String</td><td><p>Nền tảng được kext hỗ trợ. </p><p>Không cần quan tâm sử dụng i386 hay x86_64<br>Mặc định là <code>Any</code></p></td></tr><tr><td align="center">BundlePath</td><td align="center">String</td><td>Đường dẫn kext đầy đủ, kể từ <code>EFI/OC/Kexts</code></td></tr><tr><td align="center">Enabled</td><td align="center">Boolean</td><td>Cho phép kext hoạt động hoặc không</td></tr><tr><td align="center">ExecutablePath</td><td align="center">String</td><td><p>Kiểm tra bằng cách nhấp chuột phải vào kext chọn <code>Show Package Contents</code> với macOS còn Windows hoặc Linux chỉ cần mở như folder thông thường. </p><p>Nếu không tồn tại <code>Contents/MacOS/&#x3C;tên kext></code> thì hãy để trống mục này</p></td></tr><tr><td align="center">MinKernel</td><td align="center">String</td><td>macOS thấp nhất cho phép kext hoạt động</td></tr><tr><td align="center">MaxKernel</td><td align="center">String</td><td>macOS cao nhất cho phép kext hoạt động</td></tr><tr><td align="center">PlistPath</td><td align="center">String</td><td>Mặc định cứ để là <code>Contents/Info.plist</code></td></tr></tbody></table>

Còn dưới đây là bảng các MinKernel và MaxKernel&#x20;

|     Phiên bản macOS     | MinKernel | MaxKernel |
| :---------------------: | :-------: | :-------: |
| macOS High Sierra 10.13 |   17.0.0  |  17.99.99 |
|    macOS Mojave 10.14   |   18.0.0  |  18.99.99 |
|   macOS Catalina 10.15  |   19.0.0  |  19.99.99 |
|     macOS Big Sur 11    |   20.0.0  |  20.99.99 |
|    macOS Monterey 12    |   21.0.0  |  21.99.99 |
|     macOS Ventura 13    |   22.0.0  |  22.99.99 |
|     macOS Sonoma 14     |   23.0.0  |  23.99.99 |

### Block

{% hint style="info" %}
Đây là mục block kext trong S/L/E thường thì không cần động tới&#x20;
{% endhint %}

### Emulate

{% hint style="info" %}
Mục này chủ yếu để Fake CPU-ID mà thôi xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/fake-cpu-id)
{% endhint %}

<table><thead><tr><th width="256.3333333333333" align="center">Key</th><th width="119" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Cpuid1Data</td><td align="center">Data</td><td>Dùng để Fake CPUID. Xem giá trị cụ thể với một số phần cứng bên dưới</td></tr><tr><td align="center">Cpuid1Mask</td><td align="center">Data</td><td>Dùng để Fake CPUID. Xem giá trị cụ thể với một số phần cứng bên dưới</td></tr><tr><td align="center">DummyPowerManagement</td><td align="center">Boolean</td><td>Hãy bật nếu dùng CPU AMD hoặc CPU Intel Celeron/Pentium nhằm tránh gặp <code>AppleIntelCPUPowerManagement</code> panic</td></tr><tr><td align="center">MinKernel</td><td align="center">String</td><td>macOS thấp nhất cho phép áp dụng thay đổi</td></tr><tr><td align="center">MaxKernel</td><td align="center">String</td><td>macOS cao nhất cho phép áp dụng thay đổi</td></tr></tbody></table>

{% hint style="info" %}
Mục MinKernel và MaxKernel tương tự như ở `kernel --> Add` xem ở trên nhé
{% endhint %}

### Force

{% hint style="info" %}
Dùng để force các kext trong S/L/E thường không cần động đến nó
{% endhint %}

### Patch

{% hint style="info" %}
Dùng để patch các kext trong S/L/E
{% endhint %}

<table><thead><tr><th width="166.33333333333331" align="center">Key</th><th width="120" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Arch</td><td align="center">String</td><td>Nền tảng được áp dụng bản vá. Không cần quan tâm sử dụng i386 hay Any</td></tr><tr><td align="center">Base</td><td align="center">String</td><td>Áp dụng với một hoặc bất kì symbol phù hợp</td></tr><tr><td align="center">Comment</td><td align="center">String</td><td>Giải thích tác dụng, để trống hoặc ghi thêm tuỳ ý</td></tr><tr><td align="center">Count</td><td align="center">Number</td><td>Số lần áp dụng khi được tìm thấy. Giá trị 0 tức không giới hạn</td></tr><tr><td align="center">Enabled</td><td align="center">Boolean</td><td>Cho phép áp dụng hoặc không</td></tr><tr><td align="center">Find</td><td align="center">Data</td><td>Giá trị cần tìm ở dạng HEX</td></tr><tr><td align="center">Identifier</td><td align="center">String</td><td>Áp dụng bản vá dành cho kext(com.apple.driver.AppleIntelI210Ethernet) hay kernel</td></tr><tr><td align="center">Limit</td><td align="center">Number</td><td>Số lần tối đa được tìm kiếm. Giá 0 tức không giới hạn</td></tr><tr><td align="center">Mask</td><td align="center">Data</td><td>Giá trị bitwise mask dành cho <code>Find</code>. Nếu sử dụng cần áp dụng tương ứng với <code>Find</code></td></tr><tr><td align="center">MaxKernel</td><td align="center">String</td><td>macOS cao nhất cho phép áp dụng thay đổi</td></tr><tr><td align="center">MinKernel</td><td align="center">String</td><td>macOS thấp nhất cho phép áp dụng thay đổi</td></tr><tr><td align="center">Replace</td><td align="center">Data</td><td>Giá trị thay thế ở dạng HEX</td></tr><tr><td align="center">ReplaceMask</td><td align="center">Data</td><td>Giá trị bitwise mask dành cho <code>Replace</code>. Nếu sử dụng cần áp dụng tương ứng với <code>Replace</code></td></tr><tr><td align="center">Skip</td><td align="center">Number</td><td>Số lần xuất hiện cần bỏ qua trước khi áp dụng. Giá trị 0 tức không bỏ qua lần nào</td></tr></tbody></table>

{% hint style="info" %}
Mục MinKernel và MaxKernel tương tự như ở `kernel --> Add` xem ở trên nhé
{% endhint %}

### Quirks

<table><thead><tr><th width="250" align="center">Key</th><th width="117.33333333333331" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">AppleCpuPmCfgLock</td><td align="center">Boolean</td><td>Vô hiệu hoá việc ghi PKG_CST_CONFIG_CONTROL (0xE2) MSR trong AppleIntelCPUPowerManagement.kext dành cho macOS 12 trở về trước</td></tr><tr><td align="center">AppleXcpmCfgLock</td><td align="center">Boolean</td><td>Vô hiệu hoá việc ghi PKG_CST_CONFIG_CONTROL (0xE2) MSR trong XNU kernel</td></tr><tr><td align="center">AppleXcpmExtraMsrs</td><td align="center">Boolean</td><td>Vô hiệu hoá nhiều quyền truy cập MSR quan trọng đối với một số CPU không hỗ trợ XCPM</td></tr><tr><td align="center">AppleXcpmForceBoost</td><td align="center">Boolean</td><td>Bắt buộc chạy hiệu suất tối đa ở chế độ XCPM nhưng chỉ phù hợp với số ít CPU Xeon và CPU Intel Alder Lake chỉ có P-Cores</td></tr><tr><td align="center">CustomPciSerialDevice</td><td align="center">Boolean</td><td>Thay đổi địa chỉ PMIO register cho các thiết bị PCI tuỳ chỉnh</td></tr><tr><td align="center">CustomSMBIOSGuid</td><td align="center">Boolean</td><td>Cần thay đổi giá trị <code>UpdateSMBIOSMode</code> tương ứng nhằm vá lỗi GUID</td></tr><tr><td align="center">DisableIoMapper</td><td align="center">Boolean</td><td>Chỉ khi VT-d không được tắt trong BIOS cần dùng bản vá này nhằm vô hiệu hoá IOMapper (VT-d) trong XNU. Giải pháp thay thế cho việc chặn bảng ACPI DMAR và vô hiệu hoá VT-d trong BIOS. Nếu không sẽ vô tình làm ảnh hưởng đến các thiết bị khác như ethernet, Wi-Fi không thể kết nối, phổ biến gặp với phần cứng Gigabyte</td></tr><tr><td align="center">DisableIoMapperMapping</td><td align="center">Boolean</td><td>Giải quyết tương thích của các thiết bị ethernet, Wi-Fi và thunderbolt khi AppleVTD được kích hoạt trên những hệ thống có bảng ACPI DMAR có một hoặc nhiều Reserved Memory Regions. Một số hệ thống khác, chỉ cần quirk khi iGPU được bật. <strong>Không sử dụng với CPU AMD và bản vá chỉ dành cho macOS Ventura 13.3 trở về sau</strong></td></tr><tr><td align="center">DisableLinkeditJettison</td><td align="center">Boolean</td><td>Cho phép <code>Lilu</code> cùng các kext khác hoạt động với hiệu suất tốt hơn mà không cần bootarg <code>keepsyms=1</code></td></tr><tr><td align="center">DisableRtcChecksum</td><td align="center">Boolean</td><td>Vô hiệu hóa checksum (0x58-0x59) trong AppleRTC. Khuyến khích dùng RTCMemoryFixup để đảm bảo hiệu quả nhất</td></tr><tr><td align="center">ExtendBTFeatureFlags</td><td align="center">Boolean</td><td>Đặt giá trị <code>FeatureFlags</code> thành 0x0F nhằm đảm bảo đầy đủ chức năng của Bluetooth, phổ biến giải quyết vấn đề về tính năng Continuity</td></tr><tr><td align="center">ExternalDiskIcons</td><td align="center">Boolean</td><td>Áp dụng biểu tượng ổ cứng bên trong cho tất cả các ổ cứng AHCI</td></tr><tr><td align="center">ForceAquantiaEthernet</td><td align="center">Boolean</td><td>Nhằm kích hoạt ethernet 10GbE của Aquantia. Yêu cầu phải tắt <code>DisableIoMapper</code>, không chặn bảng ACPI DMAR và VT-d cần được bật trong BIOS. Hướng dẫn chi tiết tại <a href="https://github.com/CaseySJ/Aquantia-macOS-Patches">Aquantia macOS Patches</a></td></tr><tr><td align="center">ForceSecureBootScheme</td><td align="center">Boolean</td><td>Chỉ dành cho khi dùng macOS trên máy ảo với <code>SecureBootModel</code> không phải là <code>x86legacy</code></td></tr><tr><td align="center">IncreasePciBarSize</td><td align="center">Boolean</td><td>Cho phép IOPCIFamily khởi động với 2GB PCI BARs. Không sử dụng trong mọi lúc có thể</td></tr><tr><td align="center">LapicKernelPanic</td><td align="center">Boolean</td><td>Vô hiệu hóa kernel panic khi ngắt LAPIC</td></tr><tr><td align="center">LegacyCommpage</td><td align="center">Boolean</td><td>Giải quyết yêu cầu SSSE3 cho CPU 64 Bit cũ dành cho OS X 10.4 đến 10.6</td></tr><tr><td align="center">PanicNoKextDump</td><td align="center">Boolean</td><td>Cho phép đọc nhật ký kernel panic nếu có từ macOS 10.13 trở đi</td></tr><tr><td align="center">PowerTimeoutKernelPanic</td><td align="center">Boolean</td><td>Giúp khắc phục kernel panic do setPowerState vì việc thay đổi trạng thái nguồn của trình điều khiển Apple và âm thanh kỹ thuật số xuất hiện trong macOS Catalina</td></tr><tr><td align="center">ProvideCurrentCpuInfo</td><td align="center">Boolean</td><td>Cung cấp thông tin về CPU chưa có sẵn cho kernel</td></tr><tr><td align="center">SetApfsTrimTimeout</td><td align="center">Number</td><td>Đặt giá trị thành <code>0</code> nếu SSD của bạn không hỗ trợ TRIM đầy đủ cho macOS, để tâm đến tuỳ chọn này khi quá trình khởi động macOS rất chậm. Chỉ dành cho macOS Mojave 10.14 trở về sau</td></tr><tr><td align="center">ThirdPartyDrives</td><td align="center">Boolean</td><td>Nhằm kích hoạt cách tính năng có sẳn ở SSD như TRIM hoặc hibernation từ macOS Catalina 10.15 trở đi</td></tr><tr><td align="center">XhciPortLimit</td><td align="center">Boolean</td><td>Loại bỏ giới hạn 15 ports mắc định của macOS. Tìm hiểu thêm tại hướng dẫn <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/usb-fix/map-usb">Mapping USB</a></td></tr></tbody></table>

### Scheme

{% hint style="info" %}
Không cần quan tâm đến mục này có thể bỏ qua
{% endhint %}

## Misc

### BlessOverride

{% hint style="info" %}
Dùng để thêm các option tại picker xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/multiboot/huong-dan-dual-boot)
{% endhint %}

### Boot

<table><thead><tr><th width="210.33333333333331" align="center">Key</th><th width="120" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">ConsoleAttributes</td><td align="center">Number</td><td>Đặt thuộc tính cụ thể cho console</td></tr><tr><td align="center">HibernateMode</td><td align="center">String</td><td>Các chế độ phát hiện hibernation được hỗ trợ như: Auto(RTC + NVRAM), RTC, NVRAM hoặc None tức không sử dụng</td></tr><tr><td align="center">HibernateSkipsPicker</td><td align="center">Boolean</td><td>Không hiển thị màn hình login khi wake từ hibernate của macOS. <strong>Khuyến khích</strong> sử dụng cùng <code>PollAppleHotKeys</code></td></tr><tr><td align="center">HideAuxiliary</td><td align="center">Boolean</td><td>Ẩn các tuỳ chọn RecoveryOS, Time Machine, Tools, Reset NVRAM chỉ còn lại các tuỳ chọn hệ điều hành. Chỉ cần nhấn <code>Space</code> tại OpenCore Picker để các tuỳ chọn này được hiển thị trở lại</td></tr><tr><td align="center">InstanceIdentifier</td><td align="center">String</td><td>Tuỳ chọn định danh cho phiên bản hiện tại của OpenCore, liên quan đến <code>.contentVisibility</code></td></tr><tr><td align="center">LauncherOption</td><td align="center">String</td><td><p>Nhằm mục đích đảm bảo OpenCore luôn được ưu tiên khởi động. Tuỳ chọn này có thể gây lỗi trên một số phần cứng và làm chậm tốc độ khởi động chút ít. Có chế độ được hỗ trợ: Full, Short (bản không đầy đủ của <code>Full</code>, dành cho những phần cứng cũ từ <code>Insyde</code>), System</p><p>Yêu cầu quirk <code>ForceBooterSignature</code> trong <a href="tim-hieu-chi-tiet-ve-config.plist.md#quirks-1">Booter - Quirks</a> được bật nếu sử dụng. </p><p><strong>Chỉ sử dụng khi đã đưa EFI vào ổ cứng</strong></p></td></tr><tr><td align="center">LauncherPath</td><td align="center">String</td><td>Đường dẫn đến tệp khởi động cần được ưu tiên khi kích hoạt <code>LauncherOption</code>. Giá trị <code>Default</code> tức <code>EFI/OC/OpenCore.efi</code></td></tr><tr><td align="center">PickerAttributes</td><td align="center">Number</td><td>Đặt thuộc tính cụ thể cho OpenCore Picker như: hỗ trợ chuột, trackpad và đọc .VolumeIcon.icns để hiện thị icon cho các tuỳ chọn boot có sẵn</td></tr><tr><td align="center">PickerAudioAssist</td><td align="center">Boolean</td><td>Đọc các tuỳ chọn có trong OpenCore Picker. Yêu cầu kích hoạt <code>Boot-chime</code> để hoạt động, xem chi tiết hướng dẫn <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui">tại đây</a></td></tr><tr><td align="center">PickerMode</td><td align="center">String</td><td>Chọn giao diện cho OpenCore Picker. Sẵn ba tuỳ chọn sau: <code>Builtin</code> (giao diện gốc của OpenCore chỉ bao gồm chữ trắng trền nền đen, luôn luôn ưu tiên sử dụng nếu các tuỳ chọn khác gặp lỗi), <code>External</code> (sử dụng giao diện với icon, background tuỳ chỉnh nhiều hơn), <code>Apple</code> (sử dụng trình quản lí khởi động của Apple, chỉ dành cho máy Mac)</td></tr><tr><td align="center">PickerVariant</td><td align="center">String</td><td>Đường dẫn đến folder chứa những icons và background kể từ <code>EFI/OC/Resources/Image</code> cho giao diện của OpenCore Picker. Chỉ cần quan tâm nếu <code>PickerMode</code> có giá trị <code>External</code>, xem chi tiết hướng dẫn <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui">tại đây</a></td></tr><tr><td align="center">PollAppleHotKeys</td><td align="center">Boolean</td><td>Thêm các phím tắt bổ sung tuỳ chọn khởi động của macOS. Chi tiết các phím tắt có sẵn bên dưới</td></tr><tr><td align="center">ShowPicker</td><td align="center">Boolean</td><td>Hiển thị OpenCore Picker nhằm cho phép lựa chọn hệ điều hành mỗi lần khởi động</td></tr><tr><td align="center">TakeoffDelay</td><td align="center">Number</td><td>Đặt thời gian delay (ms) trước khi thực hiện các phím tắt hoặc lựa chọn khởi động</td></tr><tr><td align="center">Timeout</td><td align="center">Number</td><td>Thời gian chờ (s) trước khi OpenCore tự động hệ điều hành mắc định</td></tr></tbody></table>

<details>

<summary>Các phím tắt bổ sung khi <code>PollAppleHotKeys</code> được bật</summary>

* `Command/Windows + C + Minus(-)`: Vô hiệu hóa kiểm tra tương thích. Tương tự bootarg `-no_compat_check` khi được thêm
* `Command/Windows + K`: Khởi động release kernel
* `Command/Windows + S`: Khởi động với chế độ single user
* `Command/Windows + S + Minus(-)`: Vô hiệu hoá `KASLR slide`, cần vô hiệu hoá `SIP`
* `Command/Windows + V:` Khởi động với chế độ `verbose`. Tương tự việc bootarg `-v` khi được thêm
* `Shift + Enter`: Khởi động với chế độ an toàn

</details>

### Debug

<table><thead><tr><th width="182.33333333333331" align="center">Key</th><th width="109" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">AppleDebug</td><td align="center">Boolean</td><td>Cho phép OpenCore đọc và ghi debug log của boot.efi. <strong>Chỉ dành cho macOS 10.15.4 hoặc mới hơn</strong></td></tr><tr><td align="center">ApplePanic</td><td align="center">Boolean</td><td>Lưu nhật kí macOS kernal panic vào phân vùng chứa OpenCore</td></tr><tr><td align="center">DisableWatchDog</td><td align="center">Boolean</td><td>Vô hiệu hoá watchdog timer với một số phần cứng khởi động chậm nhằm đảm bảo quá trình vẫn được tiếp tục</td></tr><tr><td align="center">DisplayDelay</td><td align="center">Number</td><td>Độ trễ (ms) sau mỗi thông tin được hiển thị ra màn hình</td></tr><tr><td align="center">DisplayLevel</td><td align="center">Number</td><td>Xác định những thông tin gỡ lỗi cần thiết. <strong>Yêu cầu sử dụng OpenCore DEBUG</strong></td></tr><tr><td align="center">LogModules</td><td align="center">String</td><td>Lọc các mục nhật kí theo module</td></tr><tr><td align="center">SysReport</td><td align="center">Boolean</td><td>Tạo thư mục <code>SysReport</code> trong phân vùng EFI chứa các báo cáo về hệ thống (ACPI, SMBIOS, codec âm thanh). <strong>Yêu cầu sử dụng OpenCore DEBUG</strong></td></tr><tr><td align="center">Target</td><td align="center">Number</td><td>Giá trị bitmask tổng xác định các thông tin cần thiết phải được lưu lại</td></tr></tbody></table>

### Entries

{% hint style="info" %}
Giúp OpenCore biết được thêm những đường dẫn khởi động từ nhiều hệ điều hành khác.
{% endhint %}

### Security

<table><thead><tr><th width="210" align="center">Key</th><th width="107.33333333333331" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">AllowSetDefault</td><td align="center">Boolean</td><td>Thêm phím tắt <code>Control/Ctrl + Enter</code> hoặc <code>Control/Ctrl + Index</code> nhằm thiết lập tuỳ chọn khởi động hệ điều hành ưu tiên (Startup Disk)</td></tr><tr><td align="center">ApECID</td><td align="center">Number</td><td>Đặt thành số nguyên bất kì khác 0 để sử dụng mã định danh Apple Secure Boot</td></tr><tr><td align="center">AuthRestart</td><td align="center">Boolean</td><td>Bỏ qua việc nhập mật khẩu khi khởi động lại macOS sử dụng FileVault 2</td></tr><tr><td align="center">BlacklistAppleUpdate</td><td align="center">Boolean</td><td>Bỏ qua các tuỳ chọn khởi động cập nhật Apple firmware khi biến <code>run-efi-updater</code> trong NVRAM không hoạt động với một số phiên bản macOS Big Sur 11,...</td></tr><tr><td align="center">DmgLoading</td><td align="center">String</td><td>Xác định DMG được load sử dụng cho macOS Recovery</td></tr><tr><td align="center">EnablePassword</td><td align="center">Boolean</td><td>Yêu cầu password để khởi động RecoveryOS, Time Machine, Tools, Reset NVRAM nhằm đảm bảo an toàn. <strong>Vẫn đang trong giai đoạn phát triển</strong></td></tr><tr><td align="center">ExposeSensitiveData</td><td align="center">Number</td><td>Giá trị bitmask tổng biểu diễn đường dẫn bộ khởi động, phiên bản OpenCore, thông tin OEM</td></tr><tr><td align="center">HaltLevel</td><td align="center">Number</td><td>Giá trị bitmask tổng tạm dừng việc thực thi của CPU khi nhận được thông báo <code>HaltLevel</code></td></tr><tr><td align="center">PasswordHash</td><td align="center">Data</td><td>Để sử dụng yêu cầu <code>EnablePassword</code> được bật</td></tr><tr><td align="center">PasswordSalt</td><td align="center">Data</td><td>Để sử dụng yêu cầu <code>EnablePassword</code> được bật</td></tr><tr><td align="center">ScanPolicy</td><td align="center">Number</td><td>Quản lí việc tìm kiếm và hiển thị các tuỳ chọn khởi động có sẵn từ ổ cứng, USB,... của OpenCore</td></tr><tr><td align="center">SecureBootModel</td><td align="center">String</td><td>Xác định giá trị dựa vào SMBIOS phục vụ Apple Secure Boot, để tắt hãy chuyển giá trị về <code>Disabled</code>. macOS sẽ không tìm thấy bản cập nhất nếu tuỳ chọn này được tắt hoặc không thể khởi động khi có can thiệp vào hệ thống nhưng tuỳ chọn này vẫn được bật. Không dành cho tất cả mọi phần cứng cùng các phiên bản macOS</td></tr><tr><td align="center">Vault</td><td align="center">String</td><td>Nhằm đảm bảo tính toàn vẹn của OpenCore, yêu cầu thêm file cần thiết tuỳ chế độ. Giá trị <code>Optional</code> tức vô hiệu hoá tính năng này</td></tr></tbody></table>

### Serial

{% hint style="info" %}
Bạn có thể bỏ qua phần này
{% endhint %}

### Tools

{% hint style="info" %}
Mục này giúp inject tools trong `config.plist`

> Bạn chỉ việc OC\_Snapshot
{% endhint %}

Chi tiết về các dòng trong mục này&#x20;

<table><thead><tr><th width="180.33333333333331" align="center">Key</th><th width="100" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Arguments</td><td align="center">String</td><td>Nền tảng được kext hỗ trợ. Không cần quan tâm sử dụng i386 hay x86_64</td></tr><tr><td align="center">Auxiliary</td><td align="center">Boolean</td><td>Ẩn tuỳ chọn công cụ khi <code>HideAuxiliary</code> tại <a href="tim-hieu-chi-tiet-ve-config.plist.md#boot">Boot</a> đã được bật</td></tr><tr><td align="center">Comment</td><td align="center">String</td><td>Giải thích tác dụng, để trống hoặc ghi thêm tuỳ ý</td></tr><tr><td align="center">Enabled</td><td align="center">Boolean</td><td>Cho phép công cụ hoạt động hoặc không</td></tr><tr><td align="center">Flavour</td><td align="center">String</td><td>Cung cấp nội dung mô tả cho tuỳ chọn khởi động</td></tr><tr><td align="center">FullNvramAccess</td><td align="center">Boolean</td><td>Vô hiệu hoá bảo vệ NVRAM của OpenRuntime trong thời gian sử dụng công cụ</td></tr><tr><td align="center">Name</td><td align="center">String</td><td>Tên công cụ sẽ hiển thị tại OpenCore Picker</td></tr><tr><td align="center">Path</td><td align="center">String</td><td>Đường dẫn công cụ đầy đủ, kể từ <code>EFI/OC/Tools</code></td></tr><tr><td align="center">RealPath</td><td align="center">Boolean</td><td>Cung cấp đường dẫn đầy đủ cho công cụ</td></tr><tr><td align="center">TextMode</td><td align="center">Boolean</td><td>Thay thế chế độ đồ hoạ mắc định bởi chế độ văn bản</td></tr></tbody></table>

## NVRAM

### Add

#### 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14

<table><thead><tr><th width="237" align="center">Key</th><th width="99.33333333333331" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">DefaultBackgroundColor</td><td align="center">Data</td><td>Tuỳ chỉnh màu nền khi dùng GUI Builin cho OpenCore Picker tuỳ thích bằng mã màu HEX</td></tr></tbody></table>

#### 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102

<table><thead><tr><th width="144.33333333333331" align="center">Key</th><th width="79" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">rtc-blacklist</td><td align="center">Data</td><td>Nhằm khắc phục lỗi RTC phổ biến gặp trên hệ thống ASUS và HP, sử dụng cùng với kext <code>RTCMemoryFixup</code></td></tr></tbody></table>

#### 7C436110-AB2A-4BBB-A880-FE41995C9F82

<table><thead><tr><th width="246" align="center">Key</th><th width="139.33333333333331" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">ForceDisplayRotationInEFI</td><td align="center">Number</td><td>Nhằm xoay màn hình theo các giá trị như 90, 180, 270 tương ứng với góc xoay theo đơn vị độ</td></tr><tr><td align="center">SystemAudioVolume</td><td align="center">Data</td><td>Mức âm lượng dành cho boot-chime và trình đọc màn hình OpenCore Picker. Yêu cầu kích hoạt <code>Boot-chime</code> để hoạt động, xem chi tiết hướng dẫn <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui">tại đây</a></td></tr><tr><td align="center">boot-args</td><td align="center">String</td><td>Cơ bản một số bootarg cần thiết ban đầu. Xem chi tiết chức năng từng bootarg <a href="boot-arguments.md">tại đây</a> để giải quyết ít lỗi nếu gặp</td></tr><tr><td align="center">csr-active-config</td><td align="center">Data</td><td>Chỉnh sửa giá trị này nhằm vô hiệu hoá những cấu hình cụ thể hoặc hoàn toàn của SIP - <code>System Integrity Protection</code><br>Xem chi tiết <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/sip-va-gatekeeper">tại đây</a></td></tr><tr><td align="center">prev-lang:kbd</td><td align="center">Data</td><td>Xác định bố cục bàn phím mặc định theo định dạng <code>lang-COUNTRY:keyboard</code> dựa vào danh sách <a href="https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt">AppleKeyboardLayouts</a><br>Có thể sử dụng Type là String với giá trị tương ứng như <code>en-US:0</code>. <strong>Không được để trống nếu sử dụng type là String</strong></td></tr><tr><td align="center">run-efi-updater</td><td align="center">String</td><td>Nhằm ngăn chặn các bản cập nhật firmware từ Apple và OpenCore không được ưu tiên khởi động dành cho các máy Mac</td></tr></tbody></table>

### Delete

<table><thead><tr><th width="178.33333333333331" align="center">Key</th><th width="124" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">LegacyOverwrite</td><td align="center">Boolean</td><td>Cho phép thay đổi giá trị các biến NVRAM từ <code>nvram.plist</code>. Chỉ dùng cho những hệ thống không có sẵn NVRAM</td></tr><tr><td align="center">LegacySchema</td><td align="center">Dictionary</td><td>Cho phép đặt một số biến NVRAM cụ thể từ dictionary của GUIDs sang array</td></tr><tr><td align="center">WriteFlash</td><td align="center">Boolean</td><td>Nhằm cấp quyền ghi các biến NVRAM vào bộ nhớ flash</td></tr></tbody></table>

## PlatformInfo

<table><thead><tr><th width="221.33333333333331" align="center">Key</th><th width="116" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Automatic</td><td align="center">Boolean</td><td>Tạo PlatformInfo dựa vào <code>Generic</code> thay vì sử dụng <code>DataHub</code>, <code>NVRAM</code> hay <code>SMBIOS</code></td></tr><tr><td align="center">CustomMemory</td><td align="center">Boolean</td><td>Sử dụng đi kèm <code>Memory</code> nhằm xác định thông tin về RAM. Chỉ hoạt động khi <code>UpdateSMBIOS</code> được bật</td></tr><tr><td align="center">UpdateDataHub</td><td align="center">Boolean</td><td>Cập nhật các giá trị của Data Hub dựa vào <code>Generic</code> hoặc <code>DataHub</code> dựa vào tuỳ chọn của thuộc tính <code>Automatic</code></td></tr><tr><td align="center">UpdateNVRAM</td><td align="center">Boolean</td><td>Cập nhật các giá trị của NVRAM dựa vào <code>Generic</code> hoặc <code>PlatformNVRAM</code> dựa vào tuỳ chọn của thuộc tính <code>Automatic</code></td></tr><tr><td align="center">UpdateSMBIOS</td><td align="center">Boolean</td><td>Áp dụng các thông tin cho SMBIOS từ <code>Generic</code> hoặc <code>SMBIOS</code> dựa vào tuỳ chọn của thuộc tính <code>Automatic</code></td></tr><tr><td align="center">UpdateSMBIOSMode</td><td align="center">String</td><td>Áp dụng thông tin phần cứng Mac lên tất cả hệ điều hành khi được khởi động qua OpenCore, có thể gây lỗi Windows và các phần mềm riêng của từng hãng không hoạt động. Để giải quyết hãy thay <code>Create</code> thành <code>Custom</code>, thay đổi này cũng là bắt buộc khi sử dụng máy tính <code>Dell</code></td></tr><tr><td align="center">UseRawUuidEncoding</td><td align="center">Boolean</td><td>Tuỳ chọn sử dụng mã hoá Big Endian(nếu bật) hay Little Endian(nếu tắt) cho SMBIOS UUID. Không ảnh hưởng đến UUID có trong DataHub hay NVRAM</td></tr></tbody></table>

### Generic

<table><thead><tr><th width="216.33333333333331" align="center">Key</th><th width="123" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">AdviseFeatures</td><td align="center">Boolean</td><td>Thêm một số bit sửa lỗi boot Windows và cài đặt macOS. Chỉ sử dụng khi phân vùng EFI nằm sau phân vùng Windows trên cùng một ổ cứng</td></tr><tr><td align="center">MLB</td><td align="center">String</td><td>Thông tin cơ bản về máy tính Mac nhằm tương thích với các dịch vụ của macOS. </td></tr><tr><td align="center">MaxBIOSVersion</td><td align="center">Boolean</td><td>Chỉ dành cho máy Mac từ Apple. Nhằm việc cập nhật firmware từ macOS Big Sur và mới hơn</td></tr><tr><td align="center">ProcessorType</td><td align="center">Number</td><td>Các giá trị có thể thay thế như <code>1537</code>, <code>3841</code>. Đa số chỉ việc để nguyên</td></tr><tr><td align="center">ROM</td><td align="center">Data</td><td>Thông tin cơ bản về máy tính Mac nhằm tương thích với các dịch vụ của macOS.</td></tr><tr><td align="center">SpoofVendor</td><td align="center">Boolean</td><td>Đặt tên hãng sản xuất thành Acidanthera. Có thể sẽ làm các phần mềm riêng của từng hãng không hoạt động</td></tr><tr><td align="center">SystemMemoryStatus</td><td align="center">String</td><td>Cung cấp thông tin hệ thống có thể nâng cấp RAM hay không. Cứ việc để mắc định</td></tr><tr><td align="center">SystemProductName</td><td align="center">Data</td><td>Thông tin cơ bản về máy tính Mac nhằm tương thích với các dịch vụ của macOS. </td></tr><tr><td align="center">SystemSerialNumber</td><td align="center">Data</td><td>Thông tin cơ bản về máy tính Mac nhằm tương thích với các dịch vụ của macOS. </td></tr><tr><td align="center">SystemUUID</td><td align="center">String</td><td>Thông tin cơ bản về máy tính Mac nhằm tương thích với các dịch vụ của macOS.</td></tr></tbody></table>

<details>

<summary>Sử dụng GenSMBIOS</summary>

Phần trên chỉ tìm hiểu cho biết thôi chứ bạn chỉ việc dùng tool GenSMBIOS không cần dùng tool thủ công

B1: Tải GenSMBIOS [tại đây](https://github.com/corpnewt/GenSMBIOS)

B2: Chạy file `GenSMBIOS.bat`

B3: Nhấn `1`

&#x20;![](<../.gitbook/assets/image (7).png>)

&#x20;B4: Nhấn `2` và kéo file `config.plist` vào

![](<../.gitbook/assets/image (8).png>)

B5: Nhấn `Enter` và nhấn `3` và nhập tên của SMBIOS cần gen vào

![](<../.gitbook/assets/image (9).png>)

B6: Ấn Enter và tận hưởng thôi

</details>

## UEFI

### APFS

{% hint style="info" %}
Phần này bạn có thể bỏ qua và chỉ việc chỉnh&#x20;

* `MinDate: -1`
* `MinVersion``: -1`
{% endhint %}

### AppleInput

{% hint style="info" %}
Phần này bạn có thể bỏ qua và không cần quan tâm
{% endhint %}

### Audio

{% hint style="info" %}
Phần này chủ yếu dùng để patch `Chime` xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui)
{% endhint %}

### ConnectDrivers

{% hint style="info" %}
Bootloader sẽ tự load hết các trình điều khiển `UEFI` có trong `/EFI/OC/Drivers/`&#x20;

> Nếu tắt tuỳ chọn này, sẽ làm chậm quá trình khởi động chút ít.
{% endhint %}

### Drivers

{% hint style="info" %}
Đây là mục inject driver trong `config.plist`&#x20;

> Bạn chỉ việc `OC_Snapshot` mà thôi
{% endhint %}

<details>

<summary>Công dụng các driver</summary>

* `OpenRuntime.efi`: Xem như phần mở rộng của OpenCore để vá lỗi boot.efi, NVRAM và quản lí bộ nhớ tốt hơn. Thay thế cho FwRuntimeServices.efi, AptioMemoryFix, OSXAptioFixDrv,…
* `OpenHFSPlus.efi`: Giúp nhận diện boot từ các phân vùng HFS+(Mac OS Extended)
* `ResetNvramEntry.efi`: Nhằm xoá và tạo mới các giá trị cần thiết trong NVRAM. **Hãy cẩn thận khi sử dụng Reset NVRAM với BIOS của Thinkpad**
* `AudioDxe.efi`: Hỗ trợ âm thanh cho uEFI. Tìm hiểu thêm tại hướng dẫn [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui)
* `CrScreenshotDxe.efi`: Dùng phím `F10` để chụp màn hình OpenCore Picker
* `HiiDatabase.efi`: Sửa lỗi khi dùng GUI cho OpenCore Picker với dòng CPU Sandy Bridge và cũ hơn
* `NvmExpressDxe.efi`: Bổ sung driver NVMe không có sẵn cho CPU Haswell hoặc cũ hơn
* `OpenCanopy.efi`: Thêm GUI cho OpenCore Picker. Tìm hiểu thêm tại hướng dẫn [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui)
* `AppleUsbKbDxe.efi + UsbMouseDxe.efi`: Chỉ dùng với CPU Sandy Bridge và cũ hơn khi hệ thống chạy DuetPkg
* `Ps2KeyboardDxe.efi + Ps2MouseDxe.efi`: Cần thêm với những desktop dùng bàn phím và chuột PS2
* `XhciDxe.efi`: Bổ sung driver XHCI không có sẵn cho CPU Sandy Bridge hoặc cũ hơn

</details>

Đây là chi tiết các dòng trong mục này

<table><thead><tr><th width="166.33333333333331" align="center">Key</th><th width="126" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">Arguments</td><td align="center">String</td><td>Các tuỳ chọn bổ sung của driver</td></tr><tr><td align="center">Comment</td><td align="center">String</td><td></td></tr><tr><td align="center">Enabled</td><td align="center">Boolean</td><td>Cho phép driver hoạt động hoặc không</td></tr><tr><td align="center">LoadEarly</td><td align="center">Boolean</td><td>Cho phép driver hoạt động trước khi NVRAM được thiết lập trong quá trình khởi động OpenCore</td></tr><tr><td align="center">Path</td><td align="center">String</td><td>Đường dẫn driver đầy đủ, kể từ <code>EFI/OC/Drivers</code></td></tr></tbody></table>

### Input

{% hint style="info" %}
Không cần quan tâm nhiều vào nó hãy bỏ qua
{% endhint %}

### Output

{% hint style="info" %}
Không cần quan tâm nhiều vào nó hãy bỏ qua
{% endhint %}

### ProtocolOverrides

<table><thead><tr><th width="182.33333333333331" align="center">Key</th><th width="121" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">PickerAudioAssist</td><td align="center">Boolean</td><td>Nhằm kích hoạt FileVault VoiceOver. Yêu cầu kích hoạt <code>Boot-chime</code> để hoạt động, xem chi tiết hướng dẫn <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui">tại đây</a></td></tr></tbody></table>

### Quirks

<table><thead><tr><th width="244" align="center">Key</th><th width="119.33333333333331" align="center">Type</th><th>Ghi chú</th></tr></thead><tbody><tr><td align="center">ActivateHpetSupport</td><td align="center">Boolean</td><td>Hỗ trợ HPET cho các bo mạch cũ như ICH6 không có sẵn</td></tr><tr><td align="center">DisableSecurityPolicy</td><td align="center">Boolean</td><td>Nhằm vô hiệu hoá các tính năng bảo mật có sẵn (Secure Boot,...). Chỉ bật nếu không thể tắt Secure Boot trong BIOS</td></tr><tr><td align="center">EnableVectorAcceleration</td><td align="center">Boolean</td><td>Bật tính năng <a href="https://en.wikipedia.org/wiki/Advanced_Vector_Extensions">AVX vector acceleration</a>(nếu CPU hỗ trợ AVX hoặc AVX-512) giúp thuật toán băm SHA-512 và SHA-384 hiệu quả hơn. Có thể không tương thích với một số laptop ví dụ như Lenovo,...</td></tr><tr><td align="center">EnableVmx</td><td align="center">Boolean</td><td>Hỗ trợ ảo hoá trong Windows với một số máy Mac khi <strong>không thể</strong> kích hoạt trong BIOS</td></tr><tr><td align="center">ExitBootServicesDelay</td><td align="center">Number</td><td>Việc thực thi mã song song giữa FileVault 2 và <code>EXIT_BOOT_SERVICE</code> làm bộ điều khiển SATA không thể truy cập được từ macOS nên cần delay 3 đến 5s để giải quyết vấn đề với các BIOS APTIO IV (cụ thể với ASUS Z87-Pro)</td></tr><tr><td align="center">ForceOcWriteFlash</td><td align="center">Boolean</td><td>Cho các biến NVRAM được quản lí bởi OpenCore được phép ghi vào bộ nhớ flash. Cần <code>Reset NVRAM</code> để đảm bảo hoạt động đầy đủ chức năng</td></tr><tr><td align="center">ForgeUefiSupport</td><td align="center">Boolean</td><td>Hỗ trợ một phần UEFI 2.x cho EFI 1.x firmware (VD: MacPro5,1)</td></tr><tr><td align="center">IgnoreInvalidFlexRatio</td><td align="center">Boolean</td><td>Các giá trị không hợp lệ trong thanh ghi <code>MSR_FLEX_RATIO</code> (0x194) MSR (cụ thể với BIOS APTIO IV) có thể gây ra lỗi khởi động macOS trên nền tảng Intel</td></tr><tr><td align="center">ReleaseUsbOwnership</td><td align="center">Boolean</td><td>Cố gắng tách quyền sở hữu bộ điều khiển USB khỏi trình điều khiển firmware. Cân nhắc bật khi lỗi khởi động do USB hoặc các thiết bị ngoại vi</td></tr><tr><td align="center">ReloadOptionRoms</td><td align="center">Boolean</td><td>Tải lại ROM tuỳ chọn của các thiết bị PCI nếu có</td></tr><tr><td align="center">RequestBootVarRouting</td><td align="center">Boolean</td><td>Ngăn tùy chọn Startup Disk trong những firmware không tương thích với các mục khởi động của macOS. Chuyển hướng <code>EFI_GLOBAL_VARIABLE_GUID</code> sang <code>OC_VENDOR_VARIABLE_GUID</code> bởi <code>OC_FIRMWARE_RUNTIME</code> có trong OpenRuntime.efi</td></tr><tr><td align="center">ResizeGpuBars</td><td align="center">Number</td><td>Cấu hình kích thước <code>PCI BAR</code> của GPU. Hầu hết khuyến khích cập nhật BIOS lên mới nhất hoặc sử dụng <code>ReBarUEFI</code> thay thế</td></tr><tr><td align="center">ResizeUsePciRbIo</td><td align="center">Boolean</td><td>Cần thiết với những hệ thống cũ đã được chỉnh sửa với <code>ReBarUEFI</code> khi triển khai Pcilo bị lỗi</td></tr><tr><td align="center">TscSyncTimeout</td><td align="center">Number</td><td>Đồng bộ hóa TSC trên một số máy trạm và máy tính xách tay khi chạy XNU kernel gỡ lỗi. Đây chỉ là giải pháp một cho vài trường hợp cụ thể, với các trường hợp khác đề xuất sử dụng VoodooTSCSync, TSCAdjustReset hoặc CpuTscSync. Không thể hoạt động ở chế độ ACPI S3</td></tr><tr><td align="center">UnblockFsConnect</td><td align="center">Boolean</td><td>Cần thiết khi không có ổ cứng nào được liệt kê bởi chế độ <code>By Driver</code></td></tr></tbody></table>

### ReservedMemory

{% hint style="info" %}
Thường dùng để fix IGPU cho Sandy Bridge
{% endhint %}

Source: [https://lzhoang2601.github.io/gathering-files/config](https://lzhoang2601.github.io/gathering-files/config)
