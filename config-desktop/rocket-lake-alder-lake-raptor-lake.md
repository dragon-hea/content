# Rocket Lake/Alder Lake/Raptor Lake

<details>

<summary>Tổng quan</summary>

* OS X thấp nhất:&#x20;
  * macOS 10.15, Catalina

<!---->

* Macos cao nhất:
  * Bản mới nhất

</details>

## Chuẩn bị&#x20;

B1: Tải propertree [tại đây](https://github.com/corpnewt/ProperTree)

B2: Tải GenSMBios [tại đây](https://github.com/corpnewt/GenSMBIOS)

B3: Tiến hành snapshot config theo hướng dẫn [tại đây](../build-efi/creat-efi.md#chinh-sua-config)

## Tiến hành&#x20;

### ACPI

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

#### ADD

{% hint style="info" %}
Phần này không cần chỉnh sửa gì&#x20;
{% endhint %}

#### Patch

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Nếu như các bạn không thể add theo bảng dưới thì mình đã có làm sẵn file [tại đây](https://www.mediafire.com/file/si6zy3vocl8uzw2/ADBGtoXDBG.plist/file)&#x20;

> Khuyến khích copy từ file này

> Copy bằng Propertree vào config.plist ==> Root ==> ACPI ==> Patch
{% endhint %}

| Comment        | String  | ADBG to XDBG      |
| -------------- | ------- | ----------------- |
| Count          | Number  | 1                 |
| Enabled        | Boolean | True              |
| Find           | Data    | 43031419 41444247 |
| Replace        | Data    | 43031419 58444247 |
| Limit          | Number  | 0                 |
| OemTableId     | Data    | 47535741 7070     |
| TableSignature | Data    | 53534454          |
| TableLength    | Number  | 0                 |
| Skip           | Number  | 0                 |

### Booter <a href="#booter" id="booter"></a>

<figure><img src="../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

| Quirk                  | Enabled | Comment                                                                                                 |
| ---------------------- | ------- | ------------------------------------------------------------------------------------------------------- |
| DevirtualiseMmio       | YES     |                                                                                                         |
| EnableWriteUnprotector | NO      |                                                                                                         |
| ProtectUefiServices    | YES     | Cần trên Z390 system                                                                                    |
| RebuildAppleMemoryMap  | YES     |                                                                                                         |
| ResizeAppleGpuBars     | -1      | Nếu như firmware của bạn có support tăng GPU Bar sizes (ví dụ Resizable BAR Support), thì set nó là `0` |
| SyncRuntimePermissions | YES     |                                                                                                         |
| SetupVirtualMap        | NO      | Nếu stuck ở [`[EB\|#LOG:EXITBS:START]`](../issue/eb-or-log-exitbs-start.md) thì `Enable` nó lên         |

### DeviceProperties <a href="#deviceproperties" id="deviceproperties"></a>

#### ADD

{% hint style="info" %}
IGPU của các dòng này không được support
{% endhint %}

> Tiếp theo chúng ta sẽ patch tới card lan Intel's I225-V 2.5GBe thường được sử dụng trên  Comet Lake boards và cao hơn

{% hint style="info" %}
Để fix được card lan này chúng ta sẽ tiến hành fake device-id theo đường dẫn sau đây `Root ==> DeviceProperties ==> PciRoot(0x0)/Pci(0x1C,0x1)/Pci(0x0,0x0) ==> device-id`
{% endhint %}

| Key       | Type | Value      |
| --------- | ---- | ---------- |
| device-id | Data | `F2150000` |

> Nếu bạn gặp issue kernel panic trên kext A`ppleIntelI210Ethernet`  đường dẫn PCI của bạn sẽ là `PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)`

{% hint style="info" %}
Bên cạnh đó phần này còn có có thể patch Audio thông qua `DeviceProperties ==> PciRoot(0x0)/Pci(0x1b,0x0) ==> layout-id` tham khảo chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc)
{% endhint %}

{% hint style="info" %}
Nếu bạn gặp màn hình đen sau khi qua giai đoạn verbose thì sẽ tiến hành patch busid hướng dẫn chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/fix-connector)
{% endhint %}

### Kernel

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

#### ADD

{% hint style="info" %}
Đây là phần để load kext bình thường không cần chỉnh
{% endhint %}

#### Emulate <a href="#emulate" id="emulate"></a>

{% hint style="info" %}
Đây là mục fake CPU ID tham khảo chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/fake-cpu-id)
{% endhint %}

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="142">Key</th><th width="85.33333333333331">Type</th><th>Vaule</th></tr></thead><tbody><tr><td>Cpuid1Data</td><td>DATA</td><td>55060A00000000000000000000000000</td></tr><tr><td>Cpuid1Mask</td><td>DATA</td><td>FFFFFFFF000000000000000000000000</td></tr></tbody></table>

#### Patch

{% hint style="info" %}
Tiếp theo chúng ta sẽ tiếp tục patch AppleIntelI210Ethernet để enable được card lan I225-V

> Tuy nhiên nó chỉ cần trên catalina và bigsur tới 11.3
{% endhint %}

| Key        | Type    | Value                                   |
| ---------- | ------- | --------------------------------------- |
| Base       | String  | \_\_Z18e1000\_set\_mac\_typeP8e1000\_hw |
| Comment    | String  | I225-V patch                            |
| Count      | Number  | 1                                       |
| Enabled    | Boolean | True                                    |
| Find       | Data    | `F2150000`                              |
| Identifier | String  | com.apple.driver.AppleIntelI210Ethernet |
| MinKernel  | String  | 19.0.0                                  |
| MaxKernel  | String  | 20.4.0                                  |
| Replace    | Data    | `F3150000`                              |

#### Quirks <a href="#quirks-3" id="quirks-3"></a>

| Quirk                   | Enabled | Comment                                             |
| ----------------------- | ------- | --------------------------------------------------- |
| DisableIoMapper         | YES     | Không cần nếu `VT-D` bị disable trong bios          |
| LapicKernelPanic        | NO      | HP sẽ cần Quirk này                                 |
| PanicNoKextDump         | YES     |                                                     |
| PowerTimeoutKernelPanic | YES     |                                                     |
| XhciPortLimit           | YES     |                                                     |
| AppleXcpmCfgLock        | YES     | Không cần nếu `CFG-Lock` được `Disabled` trong bios |
| ProvideCurrentCpuInfo   | YES     |                                                     |

#### Scheme <a href="#scheme" id="scheme"></a>

{% hint style="info" %}
Liên quan đến các hệ thống Legacy
{% endhint %}

### Misc <a href="#misc" id="misc"></a>

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

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

* Networking-Specific

| boot-args   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **e1000=0** | <p>Disable <code>com.apple.DriverKit-AppleEthernetE1000</code> (Apple's DEXT driver) từ việc thích hợp với I225 , khiênshếno Apple's I225 kext driver được load để thay thế</p><p><br>ARG này là tuỳ chọn với hầu hết các main do nó tương thích với DEXT drive. Tuy nhiên nó là cần thiết cho Gigabyte và several boards khác, cái mà chỉ có thể dùng kext vì  DEXT driver gây issue.</p><p><br>Bạn không cần nếu như main không có I225-V NIC.<br><br>trên macOS 12.2.1 trở xuống sử dụng <code>dk.e1000=0</code> để thay thế</p> |

* Arg GPU:

| boot-args            | Description                                                                                                                                                                                               |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **agdpmod=pikera**   | Sử dụng để tắt board ID checks trên Navi GPUs (RX 5000 & 6000 series) nếu không sử dụng bạn sẽ nhận được 1 màn hình đen và chẳng có gì khác ngoài nó Không sử dụng nó nếu gpu của bạn không phải Navi GPU |
| **-radcodec**        | Cho phép các amd GPU không hỗ trợ chính thức sử dụng Hardware Video Encoder                                                                                                                               |
| **radpg=15**         | sử dụng để disable power-gating modes, Hữu ích cho GPU AMD                                                                                                                                                |
| **unfairgva=1**      | Sử dụng để fix hardware DRM support trên các AMD GPUs được hỗ trợ                                                                                                                                         |
| **nvda\_drv\_vrl=1** | Enable web driver cho Nvidia                                                                                                                                                                              |
| **-wegnoigpu**       | dùng để disable IGPU. Vẫn khuyến khích disbale bằng bios. Nếu disable bằng bios rồi thì bạn không cần arg này                                                                                             |

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

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

> Dùng SMBIOS gen để generate các smbios

| SMBIOS     | Comment      |
| ---------- | ------------ |
| MacPro7,1  |              |
| iMacPro1,1 | Khuyến khích |

Chạy gen smbios chọn 1 để download MacSerial và chọn 3 để select SMBIOS. kết quả sẽ ra tương tự như sau:

```
  #######################################################
 #               MacPro7,1 SMBIOS Info                  #
#######################################################

Type:         MacPro7,1
Serial:       C02XG0FDH7JY
Board Serial: C02839303QXH69FJA
SmUUID:       DBB364D6-44B2-4A02-B922-AB4396F16DA8
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

| Output            | Value | Comment                                                                                                                                                                                               |
| ----------------- | ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UIScale           | `0`   | <p><code>0</code>  sẽ tự set resolution<br><code>-1</code> sẽ để nó không thay đổi<br><code>1</code> cho 1x scaling, cho display bình thường<br><code>2</code> cho 2x scaling, cho HiDPI displays</p> |
| ProvideConsoleGop | YES   |                                                                                                                                                                                                       |

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

{% hint style="danger" %}
Nếu Bios của bạn có thể disable được IGPU thì hãy disable IGPU đi. Nó thường sẽ có tên là

* Internal Garaphics
* iGPU Multi Monitor
{% endhint %}
