# Creat EFI

## Chuẩn bị

B1: Tải xuống `OpenCorePkg` [tại đây](https://github.com/acidanthera/OpenCorePkg/releases)

B2: Chọn thư mục `IA32` hoặc `X64`

* &#x20;4GB RAM chọn bản 32bit (IA32)
* 4GB RAM chọn bản 64 bit (X64)

B3: các bạn cần loại bỏ 1 số mục và chỉ giữ lại những mục sau đây

* Driver :
  * OpenRuntime
  * [HFSPlus](https://github.com/JrCs/CloverGrowerPro/blob/master/Files/HFSPlus/X64/HFSPlus.efi?raw=true)
    * Có thể dùng OpenHFS để thay thế
  * ResetNvramEntry
  * OpenCanopy (nếu dùng [gui](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/cosmetics/tao-gui) thì thêm driver này không dùng có thể xoá đi)
* Tool
  * Xóa tất cả hoặc chừa lại OpenShell
* OpenCore.efi
* ACPI
* Kext
* Boot

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-12-at-23.34.21.png?w=1024)

B4: Vào file docs và copy file `sample.plist` sau đó đổi tên nó thành `config.plist`

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-12-at-23.41.10.png?w=1024)

## Thêm các kext

{% hint style="info" %}
Thêm các kext sau vào EFI ==> OC ==> Kexts xem chi tiết [tại đây](../bat-dau-voi-acpi/tim-hieu-ve-kext.md)
{% endhint %}

## Thêm các file SSDT

{% hint style="info" %}
Các bạn chỉ cần mỗi SSDT-EC để boot. nhưng để hoạt động tốt bạn cần nhiều hơn thế nữa, xem chi tiết về SSDT [tại đây](../bat-dau-voi-acpi/ssdt-recomend.md)
{% endhint %}

## Chỉnh sửa config

B1: Mở file `config.plist` bằng [ProperTree](https://github.com/corpnewt/ProperTree)&#x20;

B2: Chọn `File ⇒ OC Snapshot`&#x20;

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.23.31.png?w=874)

> Xem cách xác định phần cứng [tại đây](../general/cach-xac-dinh-phan-cung.md) sau đó setting config theo hướng dẫn

#### Desktop

<table><thead><tr><th width="341">CODE NAME</th><th width="217.33333333333331">SERIES</th><th>PHÁT HÀNH</th></tr></thead><tbody><tr><td><a href="../config-desktop/yonah-conroe-and-penryn.md">Yonah, Conroe and Penryn</a></td><td>E8XXX, Q9XXX</td><td>2006-2009</td></tr><tr><td><a href="../config-desktop/lynnfield-and-clarkdale.md">Lynnfield and Clarkdale</a></td><td>5XX-8XX</td><td>2010</td></tr><tr><td><a href="../config-desktop/sandy-bridge.md">Sandy Bridge</a></td><td>2XXX</td><td>2011</td></tr><tr><td><a href="../config-desktop/ivy-bridge.md">Ivy Bridge</a></td><td>3XXX</td><td>2012</td></tr><tr><td><a href="../config-desktop/haswell-and-broadwell.md">Haswell-Broadwell</a></td><td>4XXX-5XXX</td><td>2013-2014</td></tr><tr><td><a href="../config-desktop/skylake.md">Skylake</a></td><td>6XXX</td><td>2015-2016</td></tr><tr><td><a href="../config-desktop/kaby-lake.md">Kaby Lake</a></td><td>7XXX</td><td>2017</td></tr><tr><td><a href="../config-desktop/coffee-lake.md">Coffee Lake</a></td><td>8XXX-9XXX</td><td>2017-2019</td></tr><tr><td><a href="../config-desktop/comet-lake.md">Comet Lake</a></td><td>10XXX</td><td>2020</td></tr><tr><td><a href="../config-desktop/rocket-lake-alder-lake-raptor-lake.md">Rocket Lake/Alder Lake/Raptor Lake</a></td><td>11XXX-13XXX</td><td>2021-2022</td></tr><tr><td><a href="../config-desktop/amd-bulldozer-15h-va-jaguar-16h.md">Bulldozer(15h) và Jaguar(16h)</a></td><td>Chi tiết <a href="https://en.wikipedia.org/wiki/List_of_AMD_processors#Bulldozer_architecture;_Bulldozer,_Piledriver,_Steamroller,_Excavator_(2011%E2%80%932017)">tại đây</a></td><td>Chi tiết <a href="https://en.wikipedia.org/wiki/List_of_AMD_processors#Bulldozer_architecture;_Bulldozer,_Piledriver,_Steamroller,_Excavator_(2011%E2%80%932017)">tại đây</a></td></tr><tr><td><a href="../config-desktop/amd-ryzen-va-threadripper-17h-and-19h.md">Ryzen và Threadripper(17h và 19h)</a></td><td>1XXX, 2XXX, 3XXX, 5XXX</td><td>2017-2020</td></tr></tbody></table>

#### &#x20;Laptop

<table><thead><tr><th width="308">CODE NAME</th><th width="228.33333333333331">SERIES</th><th>PHÁT HÀNH</th></tr></thead><tbody><tr><td><a href="../config-laptop/clarksfield-and-arrandale.md">Clarksfield and Arrandale</a></td><td>3XX-9XX</td><td>2010</td></tr><tr><td><a href="../config-laptop/sandy-bridge.md">Sandy Bridge</a></td><td>2XXX</td><td>2011</td></tr><tr><td><a href="../config-laptop/ivy-bridge.md">Ivy Bridge</a></td><td>3XXX</td><td>2012</td></tr><tr><td><a href="../config-laptop/haswell.md">Haswell</a></td><td>4XXX</td><td>2013-2014</td></tr><tr><td><a href="../config-laptop/broadwell.md">Broadwell</a></td><td>5XXX</td><td>2014-2015</td></tr><tr><td><a href="../config-laptop/skylake.md">Skylake</a></td><td>6XXX</td><td>2015-2016</td></tr><tr><td><a href="../config-laptop/kaby-lake-and-amber-lake-y.md">Kaby Lake and Amber Lake</a></td><td>7XXX</td><td>2017</td></tr><tr><td><a href="../config-laptop/coffee-lake-and-whiskey-lake.md">Coffee Lake and Whiskey Lake</a></td><td>8XXX</td><td>2017-2018</td></tr><tr><td><a href="../config-laptop/coffee-lake-plus-and-comet-lake.md">Coffee Lake Plus and Comet Lake</a></td><td>9XXX-10XXX</td><td>2019-2020</td></tr><tr><td><a href="../config-laptop/icelake.md">Ice Lake</a></td><td>10XXX</td><td>2019-2020</td></tr><tr><td><a href="../config-laptop/amd-ryzen.md">AMD Ryzen</a></td><td>1XXX, 2XXX, 3XXX, 5XXX</td><td>2017-2020</td></tr></tbody></table>

<details>

<summary>Một số setting cho laptop</summary>

**HP**:

* Kernel -> Quirks -> LapicKernelPanic -> True
  * Nếu tắt quirk này bạn sẽ bị kernel panic ngay LAPIC
* UEFI -> Quirks -> UnblockFsConnect -> True

**Dell**:

Cho skylake và mới hơn

* Kernel -> Quirk -> CustomSMBIOSGuid -> True
* PlatformInfo -> UpdateSMBIOSMode -> Custom

</details>

<details>

<summary>Chỉnh sửa GUI</summary>

Nếu bạn muốn tạo GUI cho picker của mình ngay lần đầu tiên khởi động cho dễ nhìn thay vì để một màn hình trắng đen thì hãy chỉnh như sau:

B1: Tải folder `Resources` [tại đây](https://www.mediafire.com/file/npk1yg65z3otgbv/Resources.rar/file)

B2: Giải nén và paste vào EFI

B3: Đảm bảo rằng bạn có `OpenCanopy.efi`

B4: Chỉnh file `config.plist` theo sau&#x20;

* Misc ⇒ Boot ⇒ PickerMode: `External`
* Misc ⇒ Boot ⇒ PickerAttributes : `1` (nếu muốn sử dụng chuột thì có thể đổi value thành `17`)
* Misc -> Boot -> PickerVariant : Chọn tùy theo sở thích&#x20;
  * `Auto` — Tự động chọn một bộ biểu tượng dựa trên màu DefaultBackground.
  * `Acidanthera\Syrah` — Bộ biểu tượng bình thường
  * `Acidanthera\GoldenGate`— Bộ biểu tượng Nouveau
  * `Acidanthera\Chardonnay`— Bộ biểu tượng vintage&#x20;

</details>

{% hint style="info" %}
Nếu các bạn muốn tìm hiểu chi tiết về chức năng của từng dòng trong file `config.plist` thì xem chi tiết [ở đây](../general/tim-hieu-chi-tiet-ve-config.plist.md)
{% endhint %}

## Chỉnh cài đặt firmware

{% hint style="danger" %}
Bảng này được trích lại từ source [https://lzhoang2601.github.io/install-macos/setup-bios](https://lzhoang2601.github.io/install-macos/setup-bios)
{% endhint %}

<table data-full-width="false"><thead><tr><th width="229.66666666666663">Tuỳ chọn</th><th width="189" align="center">Giá trị</th><th>Ghi chú</th></tr></thead><tbody><tr><td>SATA Mode</td><td align="center"><code>AHCI</code></td><td>Một số phần cứng không cho phép chỉnh sửa nếu mắc định không phải AHCI cần sử dụng thêm kext <a href="https://github.com/lzhoang2601/lzhoang2601/releases/download/0.0.4/CtlnaAHCIPort-3.4.1.zip">CtlnaAHCIPort</a></td></tr><tr><td>Secure Boot</td><td align="center"><code>Disabled</code></td><td>N/A</td></tr><tr><td>OS Type</td><td align="center"><code>Windows 8.1/10 UEFI Mode</code> hoặc <code>Other OS</code></td><td>N/A</td></tr><tr><td>Fast Boot</td><td align="center"><code>Disabled</code></td><td>N/A</td></tr><tr><td>Serial/COM Port</td><td align="center"><code>Disabled</code></td><td>N/A</td></tr><tr><td>Parallel Port</td><td align="center"><code>Disabled</code></td><td>N/A</td></tr><tr><td>Compatibility Support Module (CSM)</td><td align="center"><code>Disabled</code></td><td>Nhằm tránh các lỗi liên quan đến GPU</td></tr><tr><td>Thunderbolt</td><td align="center"><code>Disabled</code></td><td>Chỉ bật khi đã cài đặt thành công bởi có thể gây một số lỗi trong lúc cài đặt macOS</td></tr><tr><td>Intel Virtualization Technology for Directed I/O (VT-d)</td><td align="center"><code>Disabled</code></td><td>Có thể đặt <code>Enabled</code> nếu <code>DisableIoMapper</code> trong <code>config.plist/Kernel/Quirks</code> được bật</td></tr><tr><td>Intel Software Guard Extensions (SGX)</td><td align="center"><code>Disabled</code></td><td>N/A</td></tr><tr><td>Intel Platform Trust Technology (PTT)</td><td align="center"><code>Disabled</code></td><td>N/A</td></tr><tr><td>Intel Virtualization Technology (VT-x)</td><td align="center"><code>Enabled</code></td><td>N/A</td></tr><tr><td>IOMMU</td><td align="center"><code>Disabled</code></td><td>Cần thiết nếu gặp lỗi không nhận USB sau khi wake từ sleep với hệ thống sử dụng CPU AMD thuộc thế hệ Renoir</td></tr><tr><td>Above 4G Decoding / Above 4G memory</td><td align="center"><code>Enabled</code></td><td>Nếu không tồn tại trong BIOS, có thể sử dụng bootarg <code>npci=0x3000</code> thay thế. Tuỳ chọn này có thể gây lỗi Wi-Fi, ethernet,... hoặc khởi động hệ điều hành gặp phổ biến với mainboard Asrock và Gigabyte</td></tr><tr><td>Resizable BAR Support</td><td align="center"><code>Enabled</code></td><td>Chỉ xuất hiện nếu <code>Above 4G Decoding</code> được bật dành cho một số mainboard thuộc dòng 400 và mới hơn. Nếu tuỳ chọn được bật cần đặt giá trị <code>ResizeAppleGpuBars</code> trong <code>config.plist/Booter</code> thành <code>0</code> còn không cần để giá trị <code>-1</code></td></tr><tr><td>MMIOH Base</td><td align="center"><code>12 TB</code> hoặc thấp hơn</td><td>Áp dụng với hệ thống sử dụng Intel HEDT</td></tr><tr><td>Hyper-Threading</td><td align="center"><code>Enabled</code></td><td>N/A</td></tr><tr><td>Execute Disable Bit</td><td align="center"><code>Enabled</code></td><td>N/A</td></tr><tr><td>EHCI/XHCI Hand-off</td><td align="center"><code>Enabled</code></td><td>N/A</td></tr><tr><td>Initial Display Output / Primary Display / Primary Graphics Adapter / Integrated Grahics Adapter</td><td align="center"></td><td>Nếu cần xuất hình từ dGPU hãy chọn <code>PCIe</code> còn nếu dùng iGPU hãy dùng <code>IGFX</code>. Sau khi chỉnh cần cắm dây xuất hình sang vị trí tương ứng nếu không chỉ nhận được một màn hình đen</td></tr><tr><td>Internal Graphics / iGPU Multi-Monitor / IGD Multi-Monitor</td><td align="center"><code>Enabled</code></td><td>Kích hoạt IGPU để xuất hình và phục vụ Intel Quick Sync. Nếu muốn dồn hết công việc xử lí cho dGPU hãy tắt tuỳ chọn này và cần sử dụng SMBIOS MacPro7,1 hoặc iMacPro1,1. Với CPU Intel thuộc thế hệ từ Rocket Lake và mới hơn thiết lập này hãy tắt nếu không cần sử dụng với các hệ điều hành khác macOS</td></tr><tr><td>CFG Lock</td><td align="center"><code>Disabled</code></td><td>macOS sẽ không khởi động nếu tuỳ chọn này được bật. Sử dụng <code>AppleCpuPmCfgLock</code>/<code>AppleXcpmCfgLock</code> trong <strong>config.plist</strong> nếu không tồn tại tuỳ chọn này hoặc dùng cách <a href="https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/cach-mod-bios">Can thiệp BIOS</a>. Cập nhật BIOS lên mới nhất cũng có thể xuất hiện tuỳ chọn này trên mainboard Gigabyte,...</td></tr><tr><td>DVMT Pre-Allocated</td><td align="center"><code>64MB</code> hoặc lớn hơn</td><td>Tìm hiểu thêm <a href="https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/gpu/patch-igpu">tại đây</a></td></tr></tbody></table>

{% hint style="warning" %}
Chú ý cho CPU `3990X`

MacOS hiện tại không hỗ trợ 64 luồng trong kernel. Tuy nhiên đối với CPU`3990X` có tới 128 luồng do đó bạn cần tắt `hyper threading` trong bios
{% endhint %}

{% hint style="info" %}
Đối với các CPU Pentium hoặc Celeron, nếu các bạn muốn hackinotsh cần phải có card đồ họa rời mới được hỗ trợ vì các iGPU của dòng này đều tạch và bắt buộc phải fake CPUID, xem chi tiết [ở đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/fake-cpu-id)
{% endhint %}

{% hint style="info" %}
Các cpu  desktop các bạn cần fake cpuid thành gen 10 theo hướng dẫn [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/fake-cpu-id)
{% endhint %}

## Check lại config

{% hint style="info" %}
Các bạn có thể check tay hoặc tham khảo cách dưới đây
{% endhint %}

> Phân này chỉ mang tính chất tham khảo

B1 : Download OpenCore Configurator [tại đây](https://mackie100projects.altervista.org/download-opencore-configurator/)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.51.42.png?w=1024)

B2: Bấm tổ hợp phím Option + C

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.52.31.png?w=1024)

B3: Chọn đời CPU

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.52.54.png?w=1024)

B4: Chọn phiên bản OpenCore

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.53.11.png?w=1024)

B5: Bật Drag and Drop

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.53.45.png?w=1024)

B6: Kéo file config vào ô

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.54.08.png?w=1024)

B7: Bấm `Check`

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.55.25.png?w=1024)

B8: Nhìn vào những mục màu vàng hoặc màu đỏ sau đó check lại các mục theo setting config ở trên

## Issue

{% hint style="info" %}
Nếu trong quá trình cài đặt có bất cứ lỗi nào bạn có thể tham khảo cách fix lỗi ở đây
{% endhint %}

* [Boot issue](https://heavietnam.ga/su-co-khoi-dong-opencore/)
  * Các vấn đề gặp phải từ khi khởi động usb cho đến trước khi chọn option boot macos
* [Kernel issue](../issue/kernel-issue.md)
  * Các vấn đề gặp phải từ khi chọn option boot macos ở picker cho đến khi vào giao diện cài đặt
* [Bigsur issue](../issue/issue-bigsur.md)
  * Các lỗi đặt trưng ở bigsur
* [Propertree issue](../issue/issue-propertree.md)
  * Các vấn đề gặp phải khi sử dụng propertree

> **Source tham khảo:** [**OpenCore Install Guide (dortania.github.io)**](https://dortania.github.io/OpenCore-Install-Guide/) **|** [**https://lzhoang2601.github.io/install-macos/setup-bios**](https://lzhoang2601.github.io/install-macos/setup-bios)
