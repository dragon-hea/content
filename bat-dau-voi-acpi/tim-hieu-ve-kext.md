# Tìm hiểu về kext

{% hint style="info" %}
Kext là gì? Kext (`Kernel Extension`) là 1 trình điểu khiển phần cứng mà bên Windows gọi là driver.
{% endhint %}

{% hint style="warning" %}
Chú ý:

Đối với linux và windows kext sẽ có dạng 1 folder bình thường. Và folder đó sẽ có `extension` là `.kext`&#x20;

Và nếu trong kext của bạn có chứa các file có extension là `.dSYM` thì bạn chỉ cần `delete` nó đi

> Bở vì những file đó chỉ dành cho những người dev cần kiểm tra và gỡ lỗi kext

Và hãy nhớ rằng hãy thêm các file đấy vào `EFI --> OC --> Kext` và phải `OC_snapshot` config bằng [propertree](https://github.com/corpnewt/ProperTree)

> hoặc `EFI --> Clover --> Kext --> Other`
{% endhint %}

## Kext bắt buộc

* [Lilu](https://github.com/acidanthera/Lilu/releases)
* Không có kext này bạn sẽ không thể boot được
  * Hơn nữa nó còn là một kext với rất nhiều plugin kext kèm theo cực kì hữu dụng
    * Ví dụ: `AppleALC`, `WhateverGreen`, `VirtualSMC` và còn rất nhiều kext khác phụ thuộc vào Lilu
* [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)
  * Giả lập SMC chip có trong macreal&#x20;
  * Không có nó macos không thể khởi động được
  * Bắt buộc trên Mac OS X 10.4 hoặc mới hơn

## VirtualSMC Plugins

{% hint style="info" %}
Các plugin này không bắt buộc nó chỉ là patch một số tình năng cho device của bạn. Khuyến khích thêm ở phần post install
{% endhint %}

{% hint style="warning" %}
Những kext này là plugin của `VirtualSMC.` Các plugin cần version `VirtualSMC` phù hợp

> Tốt nhất cứ tải `VirtualSMC` bản mới nhất
{% endhint %}

* SMCProcessor.kext
  * Sử dụng cho theo dỗi nhiệt độ CPU intel
* [SMCAMDProcessor.kext](https://github.com/trulyspinach/SMCAMDProcessor)
  * Sử dụng để theo dỗi nhiệt độ trên AMD CPU
  * Kext vẫn đang trong quá tình phát triển
    * Có thể không ổn định
  * Yêu cầu phải có kext [AMDRyzenCPUPowerManagement.kext](https://github.com/trulyspinach/SMCAMDProcessor/releases)
  * Yêu cầu macos 10.13 và mới hơn
* [SMCRadeonGPU.kext](https://github.com/aluveitie/RadeonSensor/releases)
  * Sử dụng để theo dõi nhiệt độ GPU AMD
  * Yêu cầu phải có kext [RadeonSensor.kext](https://github.com/aluveitie/RadeonSensor/releases)
  * Yêu cầu Macos 11 và mới hơn
* SMCSuperIO.kext
  * Sử dụng cho theo dõi tốc độ quạt trên CPU intel
  * Không hỗ trợ AMD
  * Yêu cầu Mac OS X 10.6 và mới hơn
* SMCLightSensor.kext
  * Sử dụng cho cảm biến ánh sáng trên laptop
  * Không sử dụng nếu bạn ko có các cảm biến ánh sáng&#x20;
    * Chẳng hạn ở desktop&#x20;
    * Có thể sinh ra lỗi nếu như bạn thêm kext nhưng ko có cảm biến&#x20;
  * SMCBatteryManager.kext
    * Sử dụng hiển thị phần trăm pin cho laptop
      * Xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/laptop-specifics/patch-pin)
    * Không sử dụng ở Desktop
    * Yêu cầu trên Mac OS X 10.7 và mới hơn
  * SMCDellSensors.kext
    * Cho phép theo dõi và quản lý quạt trên các thiết bị dell có support System Management Mode (SMM)
    * Không sử dụng kext này trên các thiết bị Dell không support System Management Mode (SMM)
      * Hầu hết các thiết bị Dell đều được hỗ trợ System Management Mode (SMM) và có thể sử dụng kext này
    * Yêu cầu trên Mac OS X 10.7 và mới hơn

## Graphics

* [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)
  * Sử dụng cho graphics patching, DRM fixes, board ID checks, framebuffer fixes,...
  * Sử dụng cho IGPU intel hoặc AMD DGPU và NVIDIA GPU
  * Cũng cần thiết khi enable backlight với `SSDT-PNLF`&#x20;
  * Yêu cầu Mac OS X 10.6 và mới hơn
* [NootedRed.kext](https://github.com/NootInc/NootedRed)
  * Sử dụng để enable AMD APU
  * Xem chi tiết ở mục phía dưới
  * Yêu càu Bigsur trở lên
  * Thường được sử dụng sau khi cài đặt (Post-install)
    * Để tránh gây lỗi

{% hint style="warning" %}
Chú ý cho [NootedRed.kext](https://github.com/NootInc/NootedRed/actions/runs/5567306035)

Danh sách hỗ trợ

> * `1xxx` to `5xxx` series
>   * Ví dụ: AMD Ryzen™ 7 **5**700G&#x20;
>   * Hoặc ví dụ khác: AMD Ryzen™ 3 **3**200G&#x20;
> * `7x30` series
>   * Ví dụ: AMD Ryzen™ 3 **7**3**30**U

Các bạn sẽ làm như sau đễ enable được AMD APU

* Xoá `WhateverGreen.kext`
* Disable `DGPU`&#x20;
  * Hướng dẫn chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/disable-dgpu-laptop)
* Thêm [NootedRed.kext](https://github.com/NootInc/NootedRed/suites/13683142420/artifacts/756252443)
{% endhint %}

{% hint style="danger" %}
Đặc biệt hãy nhớ rằng không bao giờ được sử dụng 2 kext ở trên cùng lúc&#x20;
{% endhint %}

## Audio

* [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases)
  * Sử dụng để patch AppleHDA.kext
    * Chắc có lẽ các bạn đã biết Patch AppleHda là phần cuối cùng của hackintosh
      * Đây cũng là phần khó nhất. Để có thể có được âm thanh hoàn hảo
      * Tuy nhiên do nó quá khó patch nên gần như không mấy ai làm được. Vì thế AppleALC ra đời
      * Thật chất AppleALC chính là patch AppleHDA tuy nhiên đây là các bản patch AppleHDA sẵn của cộng đồng được inject thông qua các layout-id nên mới cần phải thử nhiều layout-id để có thể patch được âm thanh
        * Xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc)
    * AMD 15h/16h có thể gặp issue với AppleALC&#x20;
    * AMD Ryzen/Threadripper có thể không có mic với AppleALC&#x20;
    * Yêu cầu OS X 10.4 và mới hơn
* [VoodooHDA.kext ](https://sourceforge.net/projects/voodoohda/)
  * Một phương pháp đơn giản hơn để patch âm thanh&#x20;
  * Được sử dụng khi các layout-id của AppleALC.kext không thể patch được âm thanh
  * Và bạn không thể patch AppleHDA
  * Âm thanh của VoodooHDA được dùng để chống cháy nên đừng kì vọng gì ở nó
    * Tuy nhiên bạn vẫn có thể tinh chỉnh lại VoodooHDA để cài thiện được chất lượng âm thanh xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/audio/patch-audio-voi-voodoohda)
      * Nếu bạn có một đôi tay cảm âmm tốt và chịu khó tinh chỉnh một cách kiên nẫn thì âm thanh cho ra có thể sánh ngang với AppleALC
  * Yêu cầu OS X 10.6 và mới hơn
* [VoodooHDA-FAT.kext](https://github.com/khronokernel/Legacy-Kexts/blob/master/FAT/Zip/VoodooHDA.kext.zip)
  * Chức năng tương tự voodoohda.kext&#x20;
  * Nhưng sử dụng cho 32bit
    * Vẫn có support cho 64bit
  * Hoàn hảo cho OS X 10.4-5 sử dụng 32bit

## Ethernet <a href="#ethernet" id="ethernet"></a>

{% hint style="info" %}
Xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/network/fix-ethernet)
{% endhint %}

## USB

* [USBInjectAll.kext](https://github.com/daliansky/OS-X-USB-Inject-All/releases)
  * Inject USB cho Intel
* [XHCI-unsupported.kext](https://github.com/daliansky/OS-X-USB-Inject-All/releases)
  * Cần cho non-native USB controller
  * AMD không cần kext này
  * để biết xem controller của bạn có cần nó không xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/usb-fix/map-usb)
* USBToolBox
  * [Tool](https://github.com/USBToolBox/tool) và [kext](https://github.com/USBToolBox/kext/releases)
  * Đây là một công cụ tuyệt vời có thể map usb ngay trên windows
    * Đặc biệt nó đọc cơ sỡ dữ liệu của windows nên có tính năng tiên đoán port
  * Map usb chưa bao giờ nhàn đến thế
  * Support cả AMD
* [XLNCUSBFIX.kext](https://cdn.discordapp.com/attachments/566705665616117760/566728101292408877/XLNCUSBFix.kext.zip)
  * Sửi dụng cho AMD system
    * Không khuyến khích trên Ryzen
  * Yêu cầu MacOS 10.13 và mới hơn

## Wifi và Bluetooth

{% hint style="info" %}
xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/network/fix-wifi-va-bluetooth)
{% endhint %}

## AMD CPU Specific kexts

* [XLNCUSBFIX.kext](https://cdn.discordapp.com/attachments/566705665616117760/566728101292408877/XLNCUSBFix.kext.zip)
  * Sửi dụng cho AMD system
    * Không khuyến khích trên Ryzen
  * Yêu cầu MacOS 10.13 và mới hơn
* [VoodooHDA.kext ](https://sourceforge.net/projects/voodoohda/)
  * Một phương pháp đơn giản hơn để patch âm thanh&#x20;
  * Được sử dụng khi các layout-id của AppleALC.kext không thể patch được âm thanh
  * Và bạn không thể patch AppleHDA
  * Âm thanh của VoodooHDA được dùng để chống cháy nên đừng kì vọng gì ở nó
    * Tuy nhiên bạn vẫn có thể tinh chỉnh lại VoodooHDA để cài thiện được chất lượng âm thanh xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/audio/patch-audio-voi-voodoohda)
      * Nếu bạn có một đôi tay cảm âmm tốt và chịu khó tinh chỉnh một cách kiên nẫn thì âm thanh cho ra có thể sánh ngang với AppleALC
  * Được sử dụng trên AMD do AppleALC không hỗ trợ hoàn hảo cho AMD
* [AMDRyzenCPUPowerManagement.kext](https://github.com/trulyspinach/SMCAMDProcessor)
  * CPU power management cho Ryzen systems
  * Đang được phát triển tương lai có thể ổn định nhưng ở thời điểm viết bài này nó vẫn chưa ổn định
  * Yêu cầu MacOS 10.13 và mới hơn
* [AMDTscSync.kext](https://github.com/naveenkrdy/AmdTscSync)
  * Cần thiết cho syncing TSC trên AMD CPU
  * Nếu không có kext này macos có thể khởi động cực kì chậm hoặc không thể khởi động

## Extras <a href="#extras" id="extras"></a>

* [AppleMCEReporterDisabler.kext](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)
  * SMBIOS cần kext này là&#x20;
    * MacPro6,1
    * MacPro7,1
    * iMacPro1,1
  * Yêu cầu MacOS 12.3 và mới hơn
    * Hầu hết cần cho AMD
    * Cần trên MacOS 10.15  và mới hơn cho dual-socket Intel systems
* [CpuTscSync.kext](https://github.com/acidanthera/CpuTscSync/releases)
  * Cần thiết cho syncing TSC trên [Intel's HEDT](https://quantrimang.com/cong-nghe/cpu-hedt-la-gi-192748) hoặc [server motherboards](https://maychuviet.vn/main-server-la-gi/)
  * Nếu không có kext này macos có thể khởi động cực kì chậm hoặc không thể khởi động
  * Yêu cầu OS X 10.8 và mới hơn
* [NVMeFix.kext](https://github.com/acidanthera/NVMeFix/releases)
  * Sử dụng để fix power management và initialization trên nvme controller không phải của apple
  * Yêu cầu MacOS 10.14 và mới hơn
* [SATA-Unsupported.kext](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/SATA-unsupported.kext.zip)
  * Nói chung kext này để mở rộng support cho các sata controller
  * Để xác định có cần kext này hay không trước hết hãy thử boot không có nó
  * BigSur trở lên thì dùng kext [CtlnaAHCIPort.kext](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/CtlnaAHCIPort.kext.zip) để thay thế
    * Catalina không cần quan tâm
* [CPUTopologyRebuild.kext](https://github.com/b00t0x/CpuTopologyRebuild)
  * Đây là một plugin đang thử nghiệm của Lilu dùng để tối ưu hoá Alder Lake's heterogeneous core configuration
  * Chỉ sử dụng trên Alder Lake
* [RestrictEvents.kext](https://github.com/acidanthera/RestrictEvents)
  * Patch các chức năng khác nhau của MacOS xem chi tiết [tại đây](https://github.com/acidanthera/RestrictEvents#boot-arguments)
* [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC)
  * Hỗ trợ macOS kernel extension cho eMMC support
  * Hiện tại chỉ support eMMC/MMC card lên đến HS200 speeds
  * Kext này hiện đang trong quá trình hoàn thiện và có thể gặp phải perfomance kém hoặc không hoạt động trên mốt số thiết bị&#x20;
    * SD card hiện không hỗ trợ tại thời điểm viết bài&#x20;
* [SATA-RAID-unsupported.kext](https://drive.google.com/drive/folders/1r5kISeTwwJ5TmQuk2zzgk14cR5H\_V1IG?usp=sharing)&#x20;
  * &#x20;Cho phép chuẩn Raid hoạt động trên mac thay vì AHCI
    * Dùng cho trường hợp bios không có tuỳ chọn ahci
      * Tuy nhiên mình không khuyến khích bạn dùng kext này thay vào đó có thể tham khảo mod bios [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/cach-mod-bios)

## Laptop Input

{% hint style="info" %}
Để biết loại keyboard và trackpad bạn đang sử dụng bạn có thể check thông qua device manager&#x20;

> Xem chi tiết [tại đây](../general/cach-xac-dinh-phan-cung.md)

Hoặc `dmesg | grep -i input` tại linux
{% endhint %}

{% hint style="warning" %}
Bàn phím laptop đa phần đều sử dụng PS2 controller

> Cho nên hầu hết bạn đều cần nó cho dù bạn sử dụng I2C, USB, hay SMBus trackpad.
{% endhint %}

### **PS2 Keyboards/Trackpads**

* [Acidanthera's VoodooPS2.kext](https://github.com/acidanthera/VoodooPS2/releases)
  * Hỗ trợ các PS2 devices
    * Ví dụ: keyboard, trackpad PS2
  * Yêu cầu MacOS 10.11 và mới hơn
    * Để hỗ trợ MagicTrackpad 2 function
* [RehabMan's VoodooPS2.kext](https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/)
  * Chức năng tương tự như [VoodooPS2.kext](https://github.com/acidanthera/VoodooPS2/releases)
  * Nhưng cho các PS2 controller cũ hơn hoặc khi bạn không muốn sử dụng VoodooInput
  * Yêu cầu MacOS 10.6 và mới hơn

### **SMBus Trackpads**

* [VoodooRMI.kext](https://github.com/VoodooSMBus/VoodooRMI/releases)
  * Hỗ trợ Synaptics SMBus trackpads
  * Yêu cầu MacOS 10.11 và mới hơn để hỗ trợ Magic trackpad 2 functions
  * Phụ thuộc vào Acidanthera's VoodooPS2
* [VoodooSMBus.kext](https://github.com/VoodooSMBus/VoodooSMBus/releases)
  * Hỗ trợ cho ELAN SMBus Trackpads
  * Yêu cầu MacOS 10.14 và mới hơn

### **I2C/USB HID Devices**

* [VoodooI2C.kext](https://github.com/VoodooI2C/VoodooI2C/releases)
  * Gắn vào I2C controllers để cho phép các plugin của kext giao tiếp với I2C trackpads
  * Các devices connect thông qua usb devices vẫn cần kext này
  * Nhưng chỉ một kext này thì không thể patch được i2c devices
    * Bắt buộc phải sử dụng kèm với 1 hoặc nhiều plugin bên dưới

> Nếu bối rối không biết thêm plugin nào có thể thêm tất cả plugin vào vẫn không sinh lỗi

<table><thead><tr><th width="211.33333333333331">Connection type</th><th width="256">Plugin</th><th>Notes</th></tr></thead><tbody><tr><td><code>Multitouch HID</code></td><td><code>VoodooI2CHID.kext</code></td><td>Có thể sử dụng với <code>I2C/USB Touchscreens</code> và <code>Trackpads</code></td></tr><tr><td><code>ELAN Proprietary</code></td><td><code>VoodooI2CElan.kext</code></td><td><code>ELAN1200+</code> yêu cầu <code>VoodooI2CHID</code> thay vì <code>VoodooI2CElan.kext</code></td></tr><tr><td><code>FTE1001 touchpad</code></td><td><code>VoodooI2CFTE.kext</code></td><td></td></tr><tr><td><code>Atmel Multitouch Protocol</code></td><td><code>VoodooI2CAtmelMXT.kext</code></td><td></td></tr><tr><td><code>Synaptics HID</code></td><td><a href="https://github.com/VoodooSMBus/VoodooRMI/releases">VoodooRMI.kext</a></td><td><ul><li>I2C Synaptic Trackpads </li><li>Chỉ yêu cầu <code>VoodooI2C</code> cho I2C mode</li></ul></td></tr><tr><td><code>Alps HID</code></td><td><a href="https://github.com/blankmac/AlpsHID/releases">AlpsHID.kext</a></td><td>Có thể sử dụng với USB hoặc I2C Alps trackpads. Hầu hết được sử dụng trên Dell laptops và một số model HP EliteBook </td></tr></tbody></table>

{% hint style="danger" %}
Hãy nhớ muốn enable trackpad cả i2c và ps2 thì điều đầu tiên các bạn cần làm đó chính là patch battery&#x20;

> Xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/laptop-specifics/patch-pin)
{% endhint %}

### Misc

* [ECEnabler.kext](https://github.com/1Revenger1/ECEnabler/releases)
  * Bỏ qua giới hạn 8 bit khi đọc EC fields
    * Giúp hỗ trợ Fix battery status trên nhiều devices
  * Yêu cầu OS X 10.7 và mới hơn
    * Không cần cho OSX 10.4 - 10.6
* [BrightnessKeys.kext](https://github.com/acidanthera/BrightnessKeys/releases)
  * Fix hotkey điều chỉnh độ sáng màn hình một cách tự động
    * Xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/laptop-specifics/fix-hotkeys)

## Thận trọng

{% hint style="info" %}
Khi lần đầu khởi động boot tôi khuyên các bạn chỉ thêm các kext cơ bản để hạn chế lỗi

> Trừ trường hợp các kext khác là bắt buộc phải có
>
> * [Lilu.kext](https://github.com/acidanthera/Lilu/releases)
> * [VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC/releases)
> * [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen/releases)
>   * Với IGPU intel hoặc DGPU
> * Hoặc [NootedRed](https://github.com/NootInc/NootedRed/actions/runs/5567306035)
>   * Nhớ là sau khi cài đặt mới sử dụng&#x20;
> * [VoodooPS2Controller.kext](https://github.com/acidanthera/VoodooPS2/releases)
>   * Laptop
> * [USBInjectAll.kext](https://github.com/daliansky/OS-X-USB-Inject-All/releases)
>   * Dùng inject usb intel system
> * [XHCI-unsupported](https://github.com/daliansky/OS-X-USB-Inject-All/releases)
>   * Dùng để enable controller usb intel non-native
> * [GenericUSBXHCI.kext](https://bitbucket.org/RehabMan/os-x-generic-usb3/downloads/)
>   * Nếu bạn có 2 controller USB
>   * Chỉ sử dụng cho AMD
> * [AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip)
>   * Monterey hoặc mới hơn
>   * Thường được sử dụng cho AMD
>   * Hoặc dual-socket Intel systems
{% endhint %}

{% hint style="info" %}
> 1 số máy có thể sẽ cần thêm [CpuTscSync](https://github.com/acidanthera/CpuTscSync/releases)
{% endhint %}

> Source tham khảo: [https://dortania.github.io/OpenCore-Install-Guide/ktext.html](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#laptop-input)
