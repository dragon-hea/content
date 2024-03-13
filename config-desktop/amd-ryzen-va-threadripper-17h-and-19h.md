# AMD Ryzen và Threadripper(17h and 19h)

<details>

<summary>Tổng quan</summary>

* OS X thấp nhất:&#x20;
  * macOS 10.13, High Sierra

<!---->

* Macos cao nhất:
  * Bản mới nhất

</details>

## Chuẩn bị&#x20;

### Với DGPU

B1: Tải propertree [tại đây](https://github.com/corpnewt/ProperTree)

B2: Tải GenSMBios [tại đây](https://github.com/corpnewt/GenSMBIOS)

B3: Tiến hành snapshot config theo hướng dẫn [tại đây](../build-efi/creat-efi.md#chinh-sua-config)

### Với AMD APU

{% hint style="warning" %}
Chỉ dành cho Ryzen CPU các dòng:

* `1xxx` to `5xxx` series
  * Ví dụ: AMD Ryzen™ 7 **5**700G&#x20;
  * Hoặc ví dụ khác: AMD Ryzen™ 3 **3**200G&#x20;
* `7x30` series
  * Ví dụ: AMD Ryzen™ 3 **7**3**30**U
{% endhint %}

B1: Tải propertree [tại đây](https://github.com/corpnewt/ProperTree)

B2: Tải `GenSMBios` [tại đây](https://github.com/corpnewt/GenSMBIOS)

B3: Tiến hành thêm kext [NootedRed](https://github.com/NootInc/NootedRed/suites/13986686419/artifacts/780151558)

B4: Xoá `Whatevergreen.kext`

B5: Disable DGPU theo hướng dẫn [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/disable-dgpu-laptop)

B6: Tiến hành snapshot config theo hướng dẫn [tại đây](../build-efi/creat-efi.md#chinh-sua-config)

## Tiến hành&#x20;

### ACPI

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

#### ADD

{% hint style="info" %}
Phần này không cần chỉnh sửa gì&#x20;
{% endhint %}

### Booter <a href="#booter" id="booter"></a>

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

#### Quirks <a href="#quirks-2" id="quirks-2"></a>

<table><thead><tr><th>Quirk</th><th width="125.33333333333331">Enabled</th><th>Comment</th></tr></thead><tbody><tr><td>DevirtualiseMmio</td><td>NO</td><td>Nếu main của bạn là TRx40, hãy bật quirk này và làm theo hướng dẫn <a href="../issue/fixing-kaslr.md">tại đây</a></td></tr><tr><td>EnableWriteUnprotector</td><td>NO</td><td></td></tr><tr><td>RebuildAppleMemoryMap</td><td>YES</td><td></td></tr><tr><td>ResizeAppleGpuBars</td><td>-1</td><td>Nếu bios của bạn support <code>increasing GPU Bar sizes</code> (ví dụ <code>Resizable BAR Support)</code>, thì set nó là <code>0</code></td></tr><tr><td>SetupVirtualMap</td><td>YES</td><td><ul><li>X570, B550, A520 và TRx40 có thể cần Disable</li><li>X470 và B450 với bản bios update là 2020 và mới hơn có thể bắt buộc disable</li></ul></td></tr><tr><td>SyncRuntimePermissions</td><td>YES</td><td></td></tr></tbody></table>

### DeviceProperties <a href="#deviceproperties" id="deviceproperties"></a>

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

#### ADD

{% hint style="info" %}
APU AMD dòng Ryzen có hỗ trợ nhưng bạn không cần quan tâm đến phần này xem chi tiết [tại đây](../build-efi/creat-efi.md). Cơ bản là không cần chỉnh sửa gì ở đây&#x20;
{% endhint %}

### Kernel

| Kernel                                                                                      | Kernel Patches                                                                              |
| ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| ![Kernel](https://dortania.github.io/OpenCore-Install-Guide/assets/img/kernel.abad83eb.png) | ![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/kernel-patch.1730a7f1.png) |

#### ADD

{% hint style="warning" %}
Nếu bạn cần dùng AMD APU thay vì DGPU

* Enable kext [NootedRed](https://github.com/NootInc/NootedRed/suites/13986686419/artifacts/780151558)&#x20;
* Disable kext `Whatevergreen`
{% endhint %}

#### Emulate <a href="#emulate" id="emulate"></a>

| Quirk                | Enabled |
| -------------------- | ------- |
| DummyPowerManagement | YES     |

#### Patch

{% hint style="info" %}
Đối với AMD chúng ta cần add patch [sau](https://github.com/AMD-OSX/AMD\_Vanilla/blob/master/patches.plist)
{% endhint %}

B1: Tải patch ở trên [về](https://github.com/AMD-OSX/AMD\_Vanilla/blob/master/patches.plist)

B2: Xoá mục `Root --> Kernel --> Patch` ở config đi&#x20;

B3: thêm mục `patch` mới tải về vào config

> Xem video bên dưới

{% embed url="https://www.flickr.com/photos/194144154@N04/53023513987/in/dateposted-public/" %}

{% hint style="warning" %}
Để ý mục giá trị dòng comment dòng nào có chuỗi `algrey - Force cpuid_cores_per_package` thì ta cần thay đổi dòng replace ở mục đó như sau
{% endhint %}

* `B8000000 0000` => `B8 <core count> 0000 0000`
* `BA000000 0000` => `BA <core count> 0000 0000`
* `BA000000 0090` => `BA <core count> 0000 0090`
* `BA000000 00` => `BA <core count> 0000 00`

{% hint style="info" %}
Mục `<core count>` sẽ dựa vào số nhân của CPU ở được tính ở dạng hex&#x20;
{% endhint %}

<details>

<summary>Cách xác định số Core</summary>

B1: Nhấn tổ hợp phím `Ctrl + Shift + Esc` để mở `Task Manager`

B2: Chọn `Performance` để xem số `core` của `CPU`

![](<../.gitbook/assets/image (105).png>)

Hình ảnh chỉ mang tính chất minh hoạ

</details>

Tiếp theo ta sẽ cần chuyển số core của CPU từ decimal sang hex theo bảng sau&#x20;

| Core Count | Hexadecimal |
| ---------- | ----------- |
| 2 Core     | `02`        |
| 4 Core     | `04`        |
| 6 Core     | `06`        |
| 8 Core     | `08`        |
| 12 Core    | `0C`        |
| 16 Core    | `10`        |
| 24 Core    | `18`        |
| 32 Core    | `20`        |
| 64 Core    | `40`        |

> Ví dụ tôi có 8 core thì tôi sẽ sửa đổi các giá trị Replace như sau

| Before           | After                  |
| ---------------- | ---------------------- |
| `B8000000 0000`  | `B8`**`08`**`00000000` |
| `BA000000 0000`  | `BA`**`08`**`00000000` |
| `BA000000 0090`  | `BA`**`08`**`00000090` |
| `BA000000 00`    | `BA`**`08`**`000000`   |

#### Quirks <a href="#quirks-3" id="quirks-3"></a>

| Quirk                   | Enabled | Comment |
| ----------------------- | ------- | ------- |
| PanicNoKextDump         | YES     |         |
| PowerTimeoutKernelPanic | YES     |         |
| XhciPortLimit           | YES     |         |
| ProvideCurrentCpuInfo   | YES     |         |

#### Scheme <a href="#scheme" id="scheme"></a>

{% hint style="info" %}
Liên quan đến các hệ thống Legacy
{% endhint %}

### Misc <a href="#misc" id="misc"></a>

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

#### Boot <a href="#boot" id="boot"></a>

| Quirk         | Enabled | Comment                                                                                                                   |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------------------------- |
| HideAuxiliary | YES     | Ẩn các option phụ trong menu boot của opencore. Để hiện các option này các bạn có thể ấn space ở trong menu boot opencore |

#### Debug <a href="#debug" id="debug"></a>

{% hint style="info" %}
Hữu ích với những người sử dụng opencore debug và để đọc lỗi khi boot gặp issue
{% endhint %}

| Quirk           | Enabled |
| --------------- | ------- |
| AppleDebug      | YES     |
| ApplePanic      | YES     |
| DisableWatchDog | YES     |
| Target          | 67      |

#### Security <a href="#security" id="security"></a>

{% hint style="info" %}
Mục này cũng quan trọng đừng bỏ qua chúng ta sẽ có những thay đổi như sau
{% endhint %}

| Quirk                | Enabled  | Comment                                                                                                                                            |
| -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| AllowSetDefault      | YES      |                                                                                                                                                    |
| BlacklistAppleUpdate | YES      |                                                                                                                                                    |
| ScanPolicy           | 0        |                                                                                                                                                    |
| SecureBootModel      | Default  | Bình thường bạn hãy set nó là `Default` Để cho OpenCore tự set theo Smbios. Tuy nhiên đối với macos catalina- thì các bạn hãy set nó là `Disabled` |
| Vault                | Optional | Đây là một option quan trọng hãy đặt nó là `Optional` . Hãy nhớ rằng nó có phân biệt chữ hoa và chữ thường                                         |

### NVRAM <a href="#nvram" id="nvram"></a>

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

#### Add <a href="#add-4" id="add-4"></a>

* 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14
  * sử dụng cho OpenCore's UI scaling&#x20;
  * Mặc định thông thường là đủ
* 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102
  * Chủ yếu để fix RTC
* 7C436110-AB2A-4BBB-A880-FE41995C9F82
  * Boot-arg chung

| boot-args       | Description                                                                                                                                         |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **-v**          | Arg này sẽ enable verbose mode. Dùng để hiện thị lỗi khi boot OpenCore                                                                              |
| **debug=0x100** | Giúp ngăn khởi động lại khi bị panic. Cho phép bạn đọc được lỗi                                                                                     |
| **keepsyms=1**  | Dùng chung với debug=0x100 để giúp bạn có thể dễ dàng đọc các lỗi kernel panic                                                                      |
| **alcid=1**     | dùng để fix audio bằng apple alc xem chi tiết       [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc) |
| **npci=0x3000** | Dùng để thay cho option `Above 4G Decoding` không có trong bios. Nếu bật tuỳ chọn này trong bios thì phải xoá  arg này đi                           |

* Arg GPU:

| boot-args            | Description                                                                                                                                                                                                                                                                                              |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **agdpmod=pikera**   | Sử dụng để tắt board ID checks trên Navi GPUs (RX 5000 & 6000 series) nếu không sử dụng bạn sẽ nhận được 1 màn hình đen và chẳng có gì khác ngoài nó Không khuyến khích sử dụng nó nếu gpu của bạn không phải Navi GPU                                                                                   |
| **-radcodec**        | Cho phép các amd GPU không hỗ trợ chính thức sử dụng Hardware Video Encoder                                                                                                                                                                                                                              |
| **radpg=15**         | sử dụng để disable power-gating modes, Hữu ích cho GPU AMD                                                                                                                                                                                                                                               |
| **unfairgva=1**      | Sử dụng để fix hardware DRM support trên các AMD GPUs được hỗ trợ                                                                                                                                                                                                                                        |
| **nvda\_drv\_vrl=1** | Enable web driver cho Nvidia                                                                                                                                                                                                                                                                             |
| **-wegnoegpu**       | <ul><li>Dùng để disbale tất cả các <code>GPU</code> trừ <code>IGPU</code></li><li><code>DGPU</code> là bắt buộc phải disbale nếu như các bạn dùng <code>AMD APU</code> xem chi tiết cách disable <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/disable-dgpu-desktop">tại đây</a></li></ul> |

* **csr-active-config**: `00000000`
  * Thiết lập sip mode mà không cần vào recovery
  * Tham khảo chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/sip-va-gatekeeper)
* **run-efi-updater**: `No`
  * Để ngăn các update firmware
* **prev-lang:kbd**: <>
  * để thiết lập ngô ngữ ban đầu khi cài đặt macos  `lang-COUNTRY:keyboard`
  * American: `en-US:0`(`656e2d55533a30`  là dạng HEX)
  * Full list keyboard: [AppleKeyboardLayouts.txt](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)
  * Hint: `prev-lang:kbd` có thể được chuyển thành string vì vậy bạn có thể điền vào `en-US:0` trực tiếp thay vì dùng hex
  * Hint 2: `prev-lang:kbd` có thể để trống (ví dụ `<>`) điều này sẽ làm xuất hiện bộ chọn ngôn ngữ khi cài đặt thay vì lần khởi động đầu tiên sau khi cài đặt

| Key           | Type   | Value   |
| ------------- | ------ | ------- |
| prev-lang:kbd | String | en-US:0 |

#### Delete <a href="#delete-3" id="delete-3"></a>

| Quirk      | Enabled |
| ---------- | ------- |
| WriteFlash | YES     |

### PlatformInfo <a href="#platforminfo" id="platforminfo"></a>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

> Dùng SMBIOS gen để generate các smbios

<table><thead><tr><th width="159.33333333333331">SMBIOS</th><th width="320">Hardware</th><th>Note</th></tr></thead><tbody><tr><td><code>MacPro7,1</code></td><td><ul><li><code>AMD Polaris</code> và mới hơn</li><li>Cũng có thể dùng cho <code>AMD APU</code></li></ul></td><td><ul><li>Dùng cho catalina trở lên</li><li>Khuyến khích sử dụng cho <code>AMD APU</code> từ Macos 11.0 trở lên</li></ul></td></tr><tr><td><code>iMacPro1,1</code></td><td><ul><li><code>NVIDIA Maxwell</code> và <code>Pascal</code></li><li>Hoặc <code>AMD Polaris</code> và mới hơn</li><li>Cũng có thể dùng cho <code>AMD APU</code></li></ul></td><td><ul><li>Dùng cho High Sierra hoặc Mojave</li><li>Nếu dùng cho <code>AMD APU</code> thì từ Macos 11.0 trở lên</li></ul></td></tr><tr><td><code>iMac14,2</code></td><td><code>NVIDIA Maxwell</code> và<code>Pascal</code></td><td>Dùng khi gặp black screen với card <code>NVIDIA</code> sau khi bạn cài đặt <code>Web Driver</code> với <code>SMBIOS iMacPro1,1</code></td></tr><tr><td><code>MacPro6,1</code></td><td><a href="https://en.wikipedia.org/wiki/Graphics_Core_Next">AMD GCN GPUs</a> </td><td>Hỗ trợ HD và R5/R7/R9 series</td></tr><tr><td><code>iMac20,1</code> </td><td><code>AMD APU</code></td><td>Dùng cho Macos 11.0 trở lên</td></tr></tbody></table>

Chạy gen smbios chọn 1 để download MacSerial và chọn 3 để select SMBIOS. kết quả sẽ ra tương tự như sau:

```
  #######################################################
 #               MacPro7,1 SMBIOS Info                 #
#######################################################

Type:         MacPro7,1
Serial:       F5KZV0JVP7QM
Board Serial: F5K9518024NK3F7JC
SmUUID:       535B897C-55F7-4D65-A8F4-40F4B96ED394
Apple ROM:    001D4F0D5E22
```

#### Generic

| Gen SMBIOS   | Platform config    |
| ------------ | ------------------ |
| Type         | SystemProductName  |
| Serial       | SystemSerialNumber |
| Board Serial | MLB                |
| SmUUID       | SystemUUID         |

Bạn có thể ghi rom dump từ gen smbios vào config. Sau khi cài đặt bạn có thể sửa giá trị này theo hướng dẫn Fixing iServices [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-iservices)

> &#x20;Chú ý rằng bằng cần một serial không hợp lệ. Để kiểm tra điều này hãy nhập serial tại trang [Apple's Check Coverage Page](https://checkcoverage.apple.com/), bạn cần nhận được thông báo "Unable to check coverage for this serial number." khi nhập serial vào trang trên&#x20;

**Automatic**: YES

* tạo PlatformInfo dựa trên Generic thay vì DataHub, NVRAM, và SMBIOS

### UEFI

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

* **ConnectDrivers**: YES
  * Giúp bắt buộc load các driver. Nếu set thành `No` thì các driver sẽ tự động được thêm vào. Tuy nhiên không phải tất cả các driver đều chạy một số driver có thể không chạy dẫn dến lỗi

#### Drivers <a href="#drivers" id="drivers"></a>

{% hint style="info" %}
không cần chỉnh sửa chỉ cần OC Snapshot
{% endhint %}

| Key       | Type    | Description                                                                                                                             |
| --------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| Path      | String  | đường dẫn đến file trực tiếp trong folder `OC/Drivers`                                                                                  |
| LoadEarly | Boolean | cho phép driver load trước khi khởi tạo nvram chỉ nên bật cho `OpenRuntime.efi` và `OpenVariableRuntimeDxe.efi`nếu sử dụng legacy nvram |
| Arguments | String  | thêm một số arguments cho các driver                                                                                                    |

#### APFS <a href="#apfs" id="apfs"></a>

{% hint style="info" %}
Mặc định OpenCore chỉ load các APFS drivers từ macOS Big Sur và mới hơn. Nếu bạn sử dụng Macos 10.15 và cũ hơn thì bạn sẽ cần chỉnh như sau
{% endhint %}

{% hint style="info" %}
Nếu bạn đang dùng macOS Sierra hoặc cũ hơn có thể dùng HFS thay thế APFS. Bạn có thể bỏ qua phần này nếu đang dùng macos cũ hơn
{% endhint %}

| macOS Version           | Min Version        | Min Date   |
| ----------------------- | ------------------ | ---------- |
| High Sierra (`10.13.6`) | `748077008000000`  | `20180621` |
| Mojave (`10.14.6`)      | `945275007000000`  | `20190820` |
| Catalina (`10.15.4`)    | `1412101001000000` | `20200306` |
| No restriction          | `-1`               | `-1`       |

#### Input <a href="#input" id="input"></a>

{% hint style="info" %}
Sử dụng keyboard cho hotkeys ở menu boot hoặc FileVault
{% endhint %}

| Quirk      | Value | Comment                              |
| ---------- | ----- | ------------------------------------ |
| KeySupport | NO    | Enable nếu bạn sử dụng hệ thống UEFI |

#### Output <a href="#output" id="output"></a>

| Output  | Value | Comment                                                                                                                                                                                               |
| ------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UIScale | `0`   | <p><code>0</code>  sẽ tự set resolution<br><code>-1</code> sẽ để nó không thay đổi<br><code>1</code> cho 1x scaling, cho display bình thường<br><code>2</code> cho 2x scaling, cho HiDPI displays</p> |

#### Quirks <a href="#quirks-4" id="quirks-4"></a>

{% hint style="info" %}
Liên quan đến các quirk về UEFI enviroment chúng ta sẽ thay đổi như sau
{% endhint %}

| Quirk            | Enabled | Comment             |
| ---------------- | ------- | ------------------- |
| UnblockFsConnect | NO      | Cần cho hệ thống HP |

#### ReservedMemory <a href="#reservedmemory" id="reservedmemory"></a>

{% hint style="info" %}
Chủ yếu cho IGPU sandybirdge. Ở hướng dẫn này chúng ta sẽ tạm không đề cập đến nó
{% endhint %}

{% hint style="warning" %}
Nếu bạn gặp lỗi sau khi cài đặt&#x20;

<img src="https://2996773327-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FKkg5oI9lF3urxLNHi996%2Fuploads%2Fcln5mMGKJXsgulFHm4lb%2F358004841_250890760902365_114690454589459202_n.jpg?alt=media&#x26;token=3e843f0b-e71f-4123-ac6c-c45b0e0e2915" alt="" data-size="original">

thì tham khảo cách fix [tại đây](https://heavietnam.gitbook.io/untitled/issue/kernel-issue#stuck-nhu-anh)
{% endhint %}
