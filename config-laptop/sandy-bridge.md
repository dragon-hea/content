# Sandy Bridge

<details>

<summary>Tổng quan</summary>

* OS X thấp nhất:&#x20;
  * OS X 10.6.7, Snow Leopard

<!---->

* Macos cao nhất:
  * macOS 10.13, High Sierra
* Note: Tuy nhiên bạn vẫn có thể dùng tool của chirs111 để patch được IGPU trên Catalina xem chi tiết [tại đây](https://github.com/chris1111/Fix-Graphics-HD-3000-Mojave-10.14)
* Note2: Hầu hết Sandy bridge đều không hỗ trợ UEFI

</details>

## Chuẩn bị:&#x20;

B1: Tải propertree [tại đây](https://github.com/corpnewt/ProperTree)

B2: Tải GenSMBios [tại đây](https://github.com/corpnewt/GenSMBIOS)

B3: Tiến hành snapshot config theo hướng dẫn [tại đây](../build-efi/creat-efi.md#chinh-sua-config)

## Tiến hành&#x20;

### ACPI

#### ADD

{% hint style="info" %}
Phần này không cần chỉnh sửa gì&#x20;
{% endhint %}

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

#### Delete <a href="#delete" id="delete"></a>

Removing CpuPm:

| Key            | Type    | Value              |
| -------------- | ------- | ------------------ |
| All            | Boolean | YES                |
| Comment        | String  | Delete CpuPm       |
| Enabled        | Boolean | YES                |
| OemTableId     | Data    | `437075506d000000` |
| TableLength    | Number  | 0                  |
| TableSignature | Data    | `53534454`         |

Removing Cpu0Ist:

| Key            | Type    | Value              |
| -------------- | ------- | ------------------ |
| All            | Boolean | YES                |
| Comment        | String  | Delete Cpu0Ist     |
| Enabled        | Boolean | YES                |
| OemTableId     | Data    | `4370753049737400` |
| TableLength    | Number  | 0                  |
| TableSignature | Data    | `53534454`         |

#### Patch

{% hint style="info" %}
Do bạn cần SSDT-XOSI nên bạn cũng sẽ cần patch renmae sau
{% endhint %}

<table><thead><tr><th width="251.33333333333331">Comment</th><th>String</th><th>Change _OSI to XOSI</th></tr></thead><tbody><tr><td>Enabled</td><td>Boolean</td><td>YES</td></tr><tr><td>Count</td><td>Number</td><td>0</td></tr><tr><td>Limit</td><td>Number</td><td>0</td></tr><tr><td>Find</td><td>Data</td><td><code>5f4f5349</code></td></tr><tr><td>Replace</td><td>Data</td><td><code>584f5349</code></td></tr></tbody></table>

### Booter <a href="#booter" id="booter"></a>

{% hint style="info" %}
Phần này không có gì để chỉnh hãy để mặc định
{% endhint %}

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

### DeviceProperties <a href="#deviceproperties" id="deviceproperties"></a>

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

#### ADD

{% hint style="info" %}
Để tìm hiểu chi tiết phần này bạn có thể tham khảo [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/patch-igpu)
{% endhint %}

> Config đương nhiên chưa có những phần này nên các bạn sẽ cần tạo ra chúng theo đường dẫn `Root ==> DeviceProperties ==> PciRoot(0x0)/Pci(0x2,0x0) ==> AAPL,snb-platform-id`
>
> &#x20;Hoặc&#x20;
>
> `Root ==> DeviceProperties ==> PciRoot(0x0)/Pci(0x2,0x0) ==>device-id`

{% hint style="info" %}
AAPL,snb-platform-id đường dùng để macos sử dụng để xác định trình điều khiển IGPU của bạn
{% endhint %}

<table><thead><tr><th width="252">AAPL,snb-platform-id</th><th>Type</th><th>Comment</th></tr></thead><tbody><tr><td><strong><code>00000100</code></strong></td><td>Laptop</td><td>Sử dụng với laptop</td></tr><tr><td><strong><code>10000300</code></strong></td><td>NUC</td><td>sử dụng với Nuc</td></tr></tbody></table>

{% hint style="info" %}
Với laptop có màn hình có độ phân giải là 1600x900 và lớn hơn sẽ cần properties dưới đây để macos hiểu rằng bạn đang sử dụng màn hình DualLink
{% endhint %}

| Key             | Type | Value          |
| --------------- | ---- | -------------- |
| AAPL00,DualLink | Data | **`01000000`** |

Cuối cùng bạn sẽ có được 1 thứ có dạng như sau

> Đây là ví dụ của HD 3000

<table><thead><tr><th>Key</th><th width="125.33333333333331">Type</th><th>Value</th></tr></thead><tbody><tr><td>AAPL,snb-platform-id</td><td>Data</td><td><strong><code>00000100</code></strong></td></tr><tr><td>AAPL00,DualLink</td><td>Data</td><td><strong><code>01000000</code></strong></td></tr></tbody></table>

{% hint style="info" %}
Một số Lưu ý

* Vga không được chỗ trợ
  * Tuy nhiên nó vẫn có thể chạy thông qua internal adapte DP to VGA
    * Nhưng rất ít main xem VGA là DP. Chúc bạn may mắn
* `HD 2000` cũng không được hỗ trợ
{% endhint %}

Tiếp theo chúng ta sẽ cần fake IMEI cho Sandybridge 7 series mainboard

> Để kiểm tra bạn có thể dùng aida 64 chi tiết [tại đây](../general/cach-xac-dinh-phan-cung.md#chipset-model-va-mainboard)
>
> > kiểm tra xem mã CPU của bạn là Intel Core ix-3xxx và chipset là Hx6x
> >
> > > ví dụ: HM65 hoặc HM67 với Core i3-3110M

> Add patch sau theo đường dẫn `Root ==> DeviceProperties ==> PciRoot(0x0)/Pci(0x16,0x0) ==> device-id`

| Key       | Type | Value      |
| --------- | ---- | ---------- |
| device-id | Data | `3A1C0000` |

{% hint style="info" %}
Bên cạnh đó phần này còn có có thể patch Audio thông qua `DeviceProperties ==> PciRoot(0x0)/Pci(0x1b,0x0) ==> layout-id` tham khảo chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc)
{% endhint %}

### Kernel

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

#### ADD

{% hint style="info" %}
Đây là phần để load kext bình thường không cần chỉnh
{% endhint %}

#### Emulate <a href="#emulate" id="emulate"></a>

{% hint style="info" %}
Đây là mục fake CPU ID tham khảo chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/fake-cpu-id)
{% endhint %}

#### Quirks <a href="#quirks-3" id="quirks-3"></a>

| Quirk                   | Enabled | Comment                                             |
| ----------------------- | ------- | --------------------------------------------------- |
| DisableIoMapper         | YES     | Không cần nếu `VT-D` bị disable trong bios          |
| LapicKernelPanic        | NO      | HP sẽ cần Quirk này                                 |
| PanicNoKextDump         | YES     |                                                     |
| PowerTimeoutKernelPanic | YES     |                                                     |
| XhciPortLimit           | YES     |                                                     |
| AppleCpuPmCfgLock       | YES     | Không cần nếu `CFG-Lock` được `Disabled` trong bios |

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

* Arg GPU:

| boot-args      | Description                                                                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **-wegnoegpu** | dùng để disable tất cả các gpu trừ igpu xem chi tiết các disable gpu [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/disable-dgpu-desktop) |

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

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

> Dùng SMBIOS gen để generate các smbios

| SMBIOS        | CPU Type                | GPU Type              | Display Size |
| ------------- | ----------------------- | --------------------- | ------------ |
| MacBookAir4,1 | Dual Core 17W           | iGPU: HD 3000         | 11"          |
| MacBookAir4,2 | Dual Core 17W           | iGPU: HD 3000         | 13"          |
| MacBookPro8,1 | Dual Core 35W           | iGPU: HD 3000         | 13"          |
| MacBookPro8,2 | Quad Core 45W(High End) | iGPU: HD 3000 + 6490M | 15"          |
| MacBookPro8,3 | Quad Core 45W(High End) | iGPU: HD 3000 + 6750M | 17"          |
| Macmini5,1    | Dual Core NUC           | iGPU: HD 3000         | N/A          |
| Macmini5,3    | Quad Core NUC           | iGPU: HD 3000         | N/A          |

{% hint style="info" %}
Nếu bạn định chạy Mojave và mới hơn thì tôi khuyến khích bạn sử dụng MacPro6,1
{% endhint %}

Chạy gen smbios chọn 1 để download MacSerial và chọn 3 để select SMBIOS. kết quả sẽ ra tương tự như sau:

```
  #######################################################
 #                MacBookPro8,2 SMBIOS Info            #
#######################################################

Type:         MacBookPro8,2
Serial:       C02KCYZLDNCW
Board Serial: C02309301QXF2FRJC
SmUUID:       A154B586-874B-4E57-A1FF-9D6E503E4580
```

#### Generic <a href="#generic" id="generic"></a>

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

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

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

| Quirk                  | Enabled | Comment             |
| ---------------------- | ------- | ------------------- |
| IgnoreInvalidFlexRatio | YES     |                     |
| UnblockFsConnect       | NO      | Cần cho hệ thống HP |
| ReleaseUsbOwnership    | YES     |                     |

#### ReservedMemory <a href="#reservedmemory" id="reservedmemory"></a>

{% hint style="info" %}
Chủ yếu cho IGPU sandybirdge. Ở hướng dẫn này chúng ta sẽ tạm không đề cập đến nó
{% endhint %}
