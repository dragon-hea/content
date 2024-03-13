# Boot Issue

{% hint style="info" %}
Các vấn đề gặp phải từ khi khởi động usb cho đến trước khi chọn option boot macos
{% endhint %}

## Stuck on a black screen before picker <a href="#stuck-on-a-black-screen-before-picker" id="stuck-on-a-black-screen-before-picker"></a>

{% hint style="info" %}
Khi gặp lỗi này bạn sẽ thấy một màn hình đên khi khởi động usb.&#x20;

Để khác phục nó bạn nên chỉnh `target` thành `67` trong config và xem file log đuợc dump ra đang ở lỗi nào
{% endhint %}

<details>

<summary>Một vài lỗi thường gặp xuất hiện trong log file</summary>

* Nếu file log không được dump bạn có thể fix theo cách sau
  * Cấu trúc `EFI` bị sai. Câu trúc đây đủ của 1 efi gồm như sau ![Directory Structure from OpenCore's DOC](https://dortania.github.io/OpenCore-Install-Guide/assets/img/oc-structure.ed7eba0d.png)
  * Hoặc có thể là do máy bạn không hỗ trợ uefi

<!---->

* Nếu có file log được dump ra thì hãy xem dòng cuối, nó sẽ hiển thị là 1 file driver bất kì nào đó đuối là `.efi` hoặc là `ASSERT`
  * Nếu nó hiển thị là `ASSERT` thì hãy liên lạc với nhà phát triển để tìm sự trợ giúp [Acidanthera’s Bugtracker](https://github.com/acidanthera/bugtracker)
  * Ngược lại thì hãy kiểm tra các phần sau đây
    * `HfsPlus.efi` gặp lỗi khi boot
      * có thể sử dụng thử  [HfsPlusLegacy.efi](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlusLegacy.efi) để thay thế
        * Khuyến khích cho các cpu không hỗ trợ `RDRAND`
          * Tức là Ivy bridge i3 và cũ hơn&#x20;
      * Hoặc dùng [VBoxHfs.efi](https://github.com/acidanthera/AppleSupportPkg/releases/tag/2.1.7) để thay thế tuy nhiên sẽ chậm hơn là `Hfs+`
    * `HiiDatabase.efi` gặp lỗi khi boot
      * có thể là firmware của bạn chưa support `HiiDatabase` thì bạn sẽ cần driver `HiiDatabase.efi`&#x20;
        * Ở trong mục driver của efi trong [opencorepkg](https://github.com/acidanthera/OpenCorePkg/releases)&#x20;
      * Hoặc là firmware của bạn đã support `HiiDatabase` lúc này sẽ bị xung đột bạn cần xóa driver `HiiDatabase.efi`

</details>

## Stuck on `no vault provided!` <a href="#stuck-on-no-vault-provided" id="stuck-on-no-vault-provided"></a>

* Set `Misc -> Security -> Vault` thành `Optional`
* Nếu bạn đã chạy tập lệnh `sign.command` thì lúc này `256 byte RSA-2048` đã được ghi vào. Bạn sẽ cần restore lại file `opencore.efi` có thể lấy nó ở thư mục [`opencorepkg`](https://github.com/acidanthera/OpenCorePkg/releases)

## Stuck on `OC: Invalid Vault mode` <a href="#stuck-on-oc-invalid-vault-mode" id="stuck-on-oc-invalid-vault-mode"></a>

{% hint style="warning" %}
Bạn hãy thật cần thận khi nhập giá trị vào mục `Misc -> Security -> Vault`&#x20;
{% endhint %}

{% hint style="danger" %}
Nhớ là nó có phần biệt chữ hoa và thường nhé **O**ptional là giá trị chính xác
{% endhint %}

## Can’t see macOS partitions <a href="#can-t-see-macos-partitions" id="can-t-see-macos-partitions"></a>

* Set `ScanPolicy: 0`
* Kiểm tra lại driver chắc chắn rằng nó đang là `HFS+`&#x20;
  * Không dùng `ApfsDriverLoader` nó đã bị loại bỏ từ version OpenCore `0.5.8+`
* Đối với các dòng hp thì set `UnblockFsConnect` thành `true` trong config
* Chỉnh `SATA Mode`: `AHCI` trong bios
* Chỉnh 1 số mục trong `UEFI -> APFS` như sau
  * `EnableJumpstart: YES`
  * `HideVerbose: NO`
  * `minDate: -1`
  * `minVersion: -1`

## Stuck on `OCB: OcScanForBootEntries failure - Not Found` <a href="#stuck-on-ocb-ocscanforbootentries-failure-not-found" id="stuck-on-ocb-ocscanforbootentries-failure-not-found"></a>

{% hint style="info" %}
Điều này do macos không quét được bất kì ổ đĩa nào set `Misc -> Security -> ScanPolicy -> 0` để khắc phục
{% endhint %}

## Stuck on `OCB: failed to match a default boot option` <a href="#stuck-on-ocb-failed-to-match-a-default-boot-option" id="stuck-on-ocb-failed-to-match-a-default-boot-option"></a>

{% hint style="info" %}
Fix như [`OCB: OcScanForBootEntries failure - Not Found`](boot-issue.md#stuck-on-ocb-ocscanforbootentries-failure-not-found)&#x20;

> Set `Misc -> Security -> ScanPolicy -> 0` để khắc phục
{% endhint %}

## Stuck on `OCB: System has no boot entries` <a href="#stuck-on-ocb-system-has-no-boot-entries" id="stuck-on-ocb-system-has-no-boot-entries"></a>

{% hint style="info" %}
Fix như [`OCB: OcScanForBootEntries failure - Not Found`](boot-issue.md#stuck-on-ocb-ocscanforbootentries-failure-not-found)&#x20;

> Set `Misc -> Security -> ScanPolicy -> 0` để khắc phục
{% endhint %}

## Stuck on `OCS: No schema for DSDT, KernelAndKextPatch, RtVariable, SMBIOS, SystemParameters...` <a href="#stuck-on-ocs-no-schema-for-dsdt-kernelandkextpatch-rtvariable-smbios-systemparameters" id="stuck-on-ocs-no-schema-for-dsdt-kernelandkextpatch-rtvariable-smbios-systemparameters"></a>

{% hint style="danger" %}
Điều này là do bạn sử dụng config clover hoặc dùng các trình configurator. Bạn sẽ cần build lại toàn bộ.&#x20;

> Đồng thời cũng không lấy các version cũ ghép với version mới.
{% endhint %}

## Stuck on `OC: Driver XXX.efi at 0 cannot be found` <a href="#stuck-on-oc-driver-xxx-efi-at-0-cannot-be-found" id="stuck-on-oc-driver-xxx-efi-at-0-cannot-be-found"></a>

{% hint style="warning" %}
Điều này là do driver đó có trong config nhưng không có trong EFI của bạn. Để khắc phục bạn sẽ tiến hành snapshot config lại theo hướng dẫn chi tiết tại đây&#x20;

> Lưu ý các mục nhập trong config có phân biệt chữ hoa và chữ thường&#x20;
{% endhint %}

## Stuck on `Failed to parse real field of type 1` <a href="#receiving-failed-to-parse-real-field-of-type-1" id="receiving-failed-to-parse-real-field-of-type-1"></a>

{% hint style="warning" %}
Điều này là do bộ công cụ chỉnh sửa config đã set 1 giá trị thành `real` thường là `xcode` để khắc phục bạn sẽ tiến hành chuyển nó thành `integer`
{% endhint %}

## Can’t select anything in the picker <a href="#can-t-select-anything-in-the-picker" id="can-t-select-anything-in-the-picker"></a>

* Disable `PollAppleHotKeys`&#x20;
* Enable `KeySupport`&#x20;
* Sau đó xóa `OpenUsbKbDxe` khỏi `config.plist -> UEFI -> Drivers`
  * Nhớ tiến hành `OC Snapshot`

<details>

<summary>Nếu làm như trên mà vẫn không hoạt động</summary>

* Disable `KeySupport`
* Sau đó add [`OpenUsbKbDxe`](https://github.com/acidanthera/OpenCorePkg/releases) vào `config.plist -> UEFI -> Drivers`
  * Có thể `OC Snapshot`
* Thiếu trình điều khiển ps2 keyboard&#x20;
  * Có thể dùng usb để thay thế
  * 1 số dòng laptop cũ thì cần add [`Ps2KeyboardDxe.efi`](https://github.com/acidanthera/OpenCorePkg/releases)

</details>

## SSDTs not being added <a href="#ssdts-not-being-added" id="ssdts-not-being-added"></a>

{% hint style="warning" %}
Đối với opencore thì table length header phải bằng với kích thước tệp
{% endhint %}

```
* Original Table Header:
*     Signature        "SSDT"
*     Length           0x0000015D (349)
*     Revision         0x02
*     Checksum         0xCF
*     OEM ID           "ACDT"
*     OEM Table ID     "SsdtEC"
*     OEM Revision     0x00001000 (4096)
*     Compiler ID      "INTL"
*     Compiler Version 0x20190509 (538510601)
// kích thước tệp là 347
```

{% hint style="info" %}
Chúng ta phải đổi `Length` thành `0x0000015B (347)`

> Đây thực sự là lỗi của iasl.&#x20;
>
> > Để khắc phục chúng ta nên sử dụng [maciasl](https://github.com/acidanthera/MaciASL/releases) của Acidanthera
{% endhint %}

## Booting OpenCore reboots to BIOS <a href="#booting-opencore-reboots-to-bios" id="booting-opencore-reboots-to-bios"></a>

{% hint style="info" %}
Do thư mục EFI không chính xác đảm bảo rằng thư mục OC và những thư mục khác đều nằm trong thư mục EFI
{% endhint %}

<details>

<summary>Sở đồ EFI</summary>

![Directory Structure from OpenCore's DOC](https://dortania.github.io/OpenCore-Install-Guide/assets/img/oc-structure.ed7eba0d.png)

</details>

## `OCABC: Incompatible OpenRuntime r4, require r10` <a href="#ocabc-incompatible-openruntime-r4-require-r10" id="ocabc-incompatible-openruntime-r4-require-r10"></a>

{% hint style="warning" %}
Bạn hãy chắc rằng `OpenRuntime.efi`, `BOOTx64.efi` và `OpenCore.efi` đều chùng 1 bản [opencorepkg](https://github.com/acidanthera/OpenCorePkg/releases)
{% endhint %}

{% hint style="danger" %}
Chú ý: F`wRuntimeServices` đã được rename thành `OpenRuntime` từ version `0.5.7+`
{% endhint %}

## `Failed to open OpenCore image – Access Denied` <a href="#failed-to-open-opencore-image-access-denied" id="failed-to-open-opencore-image-access-denied"></a>

{% hint style="info" %}
Trên các firmwares của những thiết bị `Microsoft Surface` việc khởi động opencore là vi phạm chính sách bảo mật ngay cả khi `secureboot` bị tắt&#x20;

> Do đó để khắc phục tình trạng này các bạn hãy enable `UEFI -> Quirks -> DisableSecurityPolicy` trong `config.plist` của các bạn
{% endhint %}

## `OC: Failed to find SB model disable halting on critical error` <a href="#oc-failed-to-find-sb-model-disable-halting-on-critical-error" id="oc-failed-to-find-sb-model-disable-halting-on-critical-error"></a>

{% hint style="warning" %}
Đây là lỗi chính tả hãy đảm bảo rằng `Misc -> Security -> SecureBootModel: Disabled`
{% endhint %}

Source tham khảo: [OpenCore Boot Issues | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/opencore-issues.html#failed-to-open-opencore-image-access-denied)
