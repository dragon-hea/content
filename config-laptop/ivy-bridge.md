# Ivy Bridge

<details>

<summary>Tổng quan</summary>

* OS X thấp nhất:&#x20;
  * OS X 10.7, Lion

<!---->

* Macos cao nhất:
  * macOS 12 Monterey
* Note: IGPU Ivy Bridge được support chính thức từ macos 11.0 trở xuống
  * Tuy nhiên bạn vẫn có thể dùng OpenCore Legacy để fix được nó trên macos 12.0 và mới hơn&#x20;

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

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

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

<table><thead><tr><th width="249.33333333333331">Comment</th><th>String</th><th>Change _OSI to XOSI</th></tr></thead><tbody><tr><td>Enabled</td><td>Boolean</td><td>YES</td></tr><tr><td>Count</td><td>Number</td><td>0</td></tr><tr><td>Limit</td><td>Number</td><td>0</td></tr><tr><td>Find</td><td>Data</td><td><code>5f4f5349</code></td></tr><tr><td>Replace</td><td>Data</td><td><code>584f5349</code></td></tr></tbody></table>

### Booter <a href="#booter" id="booter"></a>

{% hint style="info" %}
Phần này không có gì để chỉnh hãy để mặc định
{% endhint %}

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

### DeviceProperties <a href="#deviceproperties" id="deviceproperties"></a>

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

#### ADD

{% hint style="info" %}
Để tìm hiểu chi tiết phần này bạn có thể tham khảo [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/patch-igpu)
{% endhint %}

> Config đương nhiên chưa có những phần này nên các bạn sẽ cần tạo ra chúng theo đường dẫn `Root ==> DeviceProperties ==> PciRoot(0x0)/Pci(0x2,0x0) ==> AAPL,ig-platform-id`
>
> &#x20;Hoặc&#x20;
>
> `Root ==> DeviceProperties ==> PciRoot(0x0)/Pci(0x2,0x0) ==>device-id`

{% hint style="info" %}
AAPL,ig-platform-id đường dùng để macos sử dụng để xác định trình điều khiển IGPU của bạn
{% endhint %}

| AAPL,ig-platform-id | Type   | Comment                                                                                                                                                      |
| ------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`03006601`**      | Laptop | Sử dụng với màn hình **1366 x 768** display hoặc thấp hơn                                                                                                    |
| **`04006601`**      | Laptop | Sử dụng với màn hình **1600 x 900** displayhoặc cao hơn, Xem thêm các patch properties bổ sung bên dưới                                                      |
| **`09006601`**      | Laptop | Được sử dụng với một số thiết bị có màn hình được kết nối qua `eDP`  (Đối lập  classical LVDS), Phải test với **03006601** và **04006601** trước khi test nó |
| **`0B006601`**      | NUC    | Được sử dụng với Intel NUC                                                                                                                                   |

{% hint style="info" %}
Một số Lưu ý

* Vga không được chỗ trợ
  * Tuy nhiên nó vẫn có thể chạy thông qua internal adapte DP to VGA
    * Nhưng rất ít main xem VGA là DP. Chúc bạn may mắn
{% endhint %}

{% hint style="info" %}
Nếu bạn dùng `04006601` là ig-platform-id của bạn thì bạn sẽ cần thêm 1 số properties sau đây
{% endhint %}

| Key                        | Type   | Value                                                                              | Explanation                                                                                                                                                                                                                                                              |
| -------------------------- | ------ | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `framebuffer-patch-enable` | Number | `1`                                                                                | Dùng để enable Frambuffer patch                                                                                                                                                                                                                                          |
| `framebuffer-memorycount`  | Number | `2`                                                                                | Do connector của ig-platform id chưa chuẩn nên ta sẽ cần patch index của `FBMemoryCount`  thành `03 006601` (1 tức là `04`  và 2 tức là `03`)                                                                                                                            |
| `framebuffer-pipecount`    | Number | `2`                                                                                | Do connector của ig-platform id chưa chuẩn nên ta sẽ cần patch index của `PipeCount` thành `03 006601` (3 tức `04` và 2 tức `03`)                                                                                                                                        |
| `framebuffer-portcount`    | Number | `4`                                                                                | Do connector của ig-platform id chưa chuẩn nên ta sẽ cần patch index của `PortCount` thành  `03 006601` (1 tức `04` và 4 tứ `03`)                                                                                                                                        |
| `framebuffer-stolenmem`    | Data   | `00000004`                                                                         | <p>Patch STOLEN memory thành 64MB (0x04000000 từ hex thành base 10 trong Bytes) hỗ trợ cho <code>03006601</code><br>xem chi tiết <a href="https://www.tonymacx86.com/threads/guide-alternative-to-the-minstolensize-patch-with-32mb-dvmt-prealloc.221506/">ở đây</a></p> |
| `framebuffer-con1-enable`  | Number | `1`                                                                                | Enable connector 1                                                                                                                                                                                                                                                       |
| `framebuffer-con1-alldata` | Data   | `02050000 00040000 07040000 03040000 00040000 81000000 04060000 00040000 81000000` | Patch `DP Connector` sử dụng `all-data method` xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector)                                                                                                                                          |

Tiếp theo chúng ta sẽ cần fake IMEI cho **IvyBridge Hybrids** 7 series mainboard

> Để kiểm tra bạn có thể dùng aida 64 chi tiết [tại đây](../general/cach-xac-dinh-phan-cung.md#chipset-model-va-mainboard)
>
> > kiểm tra xem mã CPU của bạn là Intel Core ix-3xxx và chipset là Hx6x
> >
> > > ví dụ: HM65 hoặc HM67 với Core i3-3110M

> Add patch sau theo đường dẫn `Root ==> DeviceProperties ==> PciRoot(0x0)/Pci(0x16,0x0) ==> device-id`

| Key       | Type | Value      |
| --------- | ---- | ---------- |
| device-id | Data | `3A1E0000` |

{% hint style="info" %}
Bên cạnh đó phần này còn có có thể patch Audio thông qua `DeviceProperties ==> PciRoot(0x0)/Pci(0x1b,0x0) ==> layout-id` tham khảo chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc)
{% endhint %}

### Kernel

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

> Dùng SMBIOS gen để generate các smbios

| SMBIOS         | CPU Type                | GPU Type                      | Display Size |
| -------------- | ----------------------- | ----------------------------- | ------------ |
| MacBookAir5,1  | Dual Core 17W           | iGPU: HD 4000                 | 11"          |
| MacBookAir5,2  | Dual Core 17W           | iGPU: HD 4000                 | 13"          |
| MacBookPro10,1 | Quad Core 45W           | iGPU: HD 4000 + dGPU: GT 650M | 15"          |
| MacBookPro10,2 | Dual Core 35W(High End) | iGPU: HD 4000                 | 13"          |
| Macmini6,1     | Dual Core NUC           | iGPU: HD 4000                 | N/A          |
| Macmini6,2     | Quad Core NUC           | iGPU: HD 4000                 | N/A          |

{% hint style="info" %}
Nếu bạn định chạy BigSur và mới hơn thì tôi khuyến khích bạn sử dụng các smbios sau đây
{% endhint %}

Big Sur:

| SMBIOS         | CPU Type      | Display Size |
| -------------- | ------------- | ------------ |
| MacBookAir6,1  | Dual Core 15W | 11"          |
| MacBookAir6,2  | Dual Core 15W | 13"          |
| MacBookPro11,1 | Dual Core 28W | 13"          |
| MacBookPro11,2 | Quad Core 45W | 15"          |
| MacBookPro11,3 | Quad Core 45W | 15"          |
| MacBookPro11,4 | Quad Core 45W | 15"          |
| MacBookPro11,5 | Quad Core 45W | 15"          |
| Macmini7,1     | NUC Systems   | N/A          |

{% hint style="info" %}
Do IGPU của Ivy Bridge không support chính thức cho monterey. Nên bạn cần phải dùng smbios của Broadwell. Ở đây mình sẽ không nói rõ các bạn có thể chỉnh theo bài config Broadwell

> hoặc Haswell monterey
{% endhint %}

Chạy gen smbios chọn 1 để download MacSerial và chọn 3 để select SMBIOS. kết quả sẽ ra tương tự như sau:

```
  #######################################################
 #               MacBookPro10,2 SMBIOS Info            #
#######################################################

Type:         MacBookPro10,2
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

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

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
