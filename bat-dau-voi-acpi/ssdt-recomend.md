# SSDT recomend

{% hint style="info" %}
Đối với hầu hết các model chỉ cần SSDT-EC là đã có thể boot lên. Tuy nhiên để sử được đầy đủ chức năng thì các bạn sẽ cần add đầy đủ các SSDT. Sau đây là 1 sô ssdt khuyến khích nên có
{% endhint %}

<details>

<summary>Công dụng của các SSDT</summary>

* **SSDT-PM** và **SSDT-PLUG**: Cung cấp quản lí năng lượng cần thiết cho CPU.
* **SSDT-EC**: Thêm EC - `Embedded Controller` có tên EC nhằm đảm bảo hoạt động cho macOS Catalina và mới hơn. Với desktop cần vô hiệu hoá Embedded Controller (EC) có sẵn còn với laptop cần để nguyên vì các chức năng liên quan(trạng thái pin, phím tắt FN,...)
* **SSDT-USBX**: Bổ sung các thuộc tính nguồn cần thiết cho USB, yêu cầu Embedded Controller (EC) tồn tại.
* **SSDT-AWAC**: Nhằm đảm bảo các mainboard 300 series và mới hơn hoạt động AWAC - `Alarm Wake ACPI Clock`. Có thể kích hoạt RTC - `Real-Time Clock` để thay thế.
* **SSDT-PMC**: Hỗ trợ NVRAM gốc cho các mainboard 300 series và mới hơn với thiết bị PMCR.
* **SSDT-RHUB**: Vô hiệu hoá các thiết bị RHUB có sẵn trên các bộ điều khiển USB nhằm đảm bảo các cổng USB được liệt kê đầy đủ trên macOS. Không cần thiết sau khi [Mapping USB](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/usb-fix/map-usb).
* **SSDT-RTC0-RANGE**: Đảm bảo thiết bị RTC tương thích với macOS.
* **SSDT-UNC**: Cần thiết với những người dùng mainboard X79/C602/X99/C612 nhằm vô hiệu hoá các thiết bị không dùng đến gây lỗi IOPCIFamily panic.
* **SSDT-PNLF**: Kích hoạt chức năng chỉnh độ sáng màn hình gốc trên laptop.
* **SSDT-HPET**: Khắc phục xung đột IRQ bắt buộc dành cho các laptop CPU Intel Broadwell trở về trước.
* **SSDT-GPI0**: Đảm bảo thiết bị GPI0 được kích hoạt cùng VoodooI2C để trackpad I2C hoạt động.
* **SSDT-XOSI**: Đánh lừa hệ thống rằng Windows đang được khởi động nhằm kích hoạt các thiết bị ngoại vi bị khoá như: trackpad,... cho macOS. Có thể là nguyên nhân gây lỗi khởi động Windows qua OpenCore.

</details>

## Desktop

### Penryn/Lynnfield/Clarkdale

* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### SandyBridge/Ivy Bridge

* [CPU-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) ( Thêm SSDT này sau khi đã cài đặt xong và bước đến giao đoạn Post-Install )
* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### Haswell/Broadwell

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### Skylake/Kaby Lake

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)

### Coffee Lake

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
* [SSDT-AWAC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-AWAC.aml) ( Hoặc dùng ssdt-time để prebuilt ssdt )
* [SSDT-PMC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PMC.aml)

### Comet Lake

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
* [SSDT-AWAC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-AWAC.aml) ( Hoặc dùng ssdt-time để prebuilt ssdt )
* [SSDT-RHUB](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-RHUB.aml)

### Rocket Lake/Alder Lake/Raptor Lake

* [SSDT-PLUG-ALT](https://github.com/luchina-gabriel/youtube-files/raw/main/SSDT-PLUG-ALT.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
* [SSDT-AWAC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-AWAC.aml) ( Hoặc dùng ssdt-time để prebuilt ssdt )
* [SSDT-RHUB](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-RHUB.aml)
* [SSDT-MCHC](https://github.com/king-dragon/opencore-tweak/raw/main/SSDT-MCHC.aml)
* [SSDT-SMBUS](https://github.com/king-dragon/opencore-tweak/raw/main/SSDT-SMBUS.aml)

### AMD (15/16h)

* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
* [SSDT-PLUG ](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml)

### AMD (17/19h)

* [SSDT-CPUR](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-CPUR.aml)
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
* [SSDT-PLUG ](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml)

## High End Desktop

### Nehalem and Westmere

* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### Sandy Bridge-E/Ivy Bridge-E

* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)
* [SSDT-UNC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-UNC.aml)

### Haswell-E/Broadwell-E

* [SSDT-PLUG ](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)
* [SSDT-RTC0-RANGE](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-RTC0-RANGE-HEDT.aml)
* [SSDT-UNC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-UNC.aml)

### Skylake-X

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
* [SSDT-RTC0-RANGE](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-RTC0-RANGE-HEDT.aml)

## Laptop

### Clarksfield and Arrandale

* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-LAPTOP.aml)
* [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PNLF.aml)
* [SSDT-IRQ](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc) ( Hoặc dùng SSDT-time để prebuilt )

### SandyBridge/Ivy Bridge

* [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) ( Thêm SSDT này sau khi cài đặt xong )
* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-LAPTOP.aml)
* [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PNLF.aml)
* [SSDT-IRQ](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc) ( Hoặc dùng SSDT-time để prebuilt )
* [SSDT-IMEI](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-IMEI.aml)

### Haswell/Broadwell

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) (hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-LAPTOP.aml)
* [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PNLF.aml)
* [SSDT-IRQ](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc) ( Hoặc dùng SSDT-time để prebuilt )
* [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/laptop-specifics/fix-trackpad) )

### Skylake/Kaby Lake

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-LAPTOP.aml)
* [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PNLF.aml)
* [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/laptop-specifics/fix-trackpad) )

### Coffee Lake/Whiskey Lake/Comet Lake

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-LAPTOP.aml)
* [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PNLF.aml)
* [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/laptop-specifics/fix-trackpad) )
* [SSDT-AWAC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-AWAC.aml) ( Hoặc dùng ssdt-time để prebuilt ssdt )
* [SSDT-PMC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PMC.aml) ( chỉ sử dụng cho coffelake gen 9 )

### Ice Lake

* [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-power-management) )
* [SSDT-EC-USBX](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-EC-USBX-LAPTOP.aml)
* [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-PNLF.aml)
* [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/laptop-specifics/fix-trackpad) )
* [SSDT-AWAC](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-AWAC.aml) ( Hoặc dùng ssdt-time để prebuilt ssdt )
* [SSDT-RHUB](https://github.com/dortania/Getting-Started-With-ACPI/raw/master/extra-files/compiled/SSDT-RHUB.aml)

### AMD Ryzen

{% hint style="info" %}
Dùng SSDT-time để build SSDT&#x20;

> Hướng dẫn chi tiết [ở đây](ssdt-time-prebuilt-ssdt.md)
{% endhint %}

> **Main source:** [**Gathering files | OpenCore Install Guide**](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#ssdts) **|** [**https://lzhoang2601.github.io/gathering-files/acpi**](https://lzhoang2601.github.io/gathering-files/acpi)
