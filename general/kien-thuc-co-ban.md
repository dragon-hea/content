# Kiến thức cơ bản

## Tìm hiểu về một số thuật ngữ

{% hint style="info" %}
Phần này sẽ giúp cho các bạn làm quen và đỡ bỡ ngỡ hơn
{% endhint %}

{% hint style="success" %}
WEG chính là kext thông dụng nhất [Whatevergreen](https://github.com/acidanthera/WhateverGreen/releases)
{% endhint %}

{% hint style="success" %}
Kext

Trước hết, kext là gì?&#x20;

> Kext (`Kernel Extension`) là 1 trình điểu khiển phần cứng mà bên Windows gọi là drivers.&#x20;

Thế thêm kext vào đâu?

> Có 3 nơi thêm kext:
>
> * O/K **(OC ⇒ Kext)**&#x20;
>   * Hoặc C/K/O **(CLOVER ⇒ Kexts ⇒ Other)**
> * L/E **(Library ⇒ Extensions)**
> * S/L/E **(System ⇒ Library ⇒ Extentions)**
>
> > Thứ tự ưu tiên O/K (C/K/O) ⇒ L/E ⇒ S/L/E&#x20;
> >
> > > Khi bỏ kext vào `L/E` hoặc `S/L/E` ta cần prebuilt kext cache
> > >
> > > Để bỏ kext vào L/E ta dùng [kext droplet](https://github.com/chris1111/Kext-Droplet-Big-Sur/releases)
> > >
> > > &#x20;để bỏ kext vào S/L/E ta có thể dùng [kext utility](https://taimienphi.vn/download-kext-utility-for-mac-34434)
> >
> > Khi bỏ kext vào O/K ta cần thêm kext vào config.plist xem caách thêm kext [tại đây](tim-hieu-chi-tiet-ve-config.plist.md#cach-oc\_snapshot)
{% endhint %}

{% hint style="success" %}
SSDT và DSDT

Trước hết SSDT và DSDT là gì&#x20;

> Ta có thể hiểu nó là các bản giao thức điều khiển thiết bị
>
> > Xem chi tiết [tại đây](https://en.wikipedia.org/wiki/Advanced\_Configuration\_and\_Power\_Interface)

Tại sao cần patch DSDT/SSDT khi hackintosh?

> * Nhưng bạn cũng cần phải biết không chỉ MacOS mới có ACPI mà tất cả các máy đều có ACPI bạn nhé
> * Mà do DSDT/SSDT của macOS được Apple quản lý theo 1 quy định chặt chẽ tối ưu cho các máy Mac,&#x20;
> * Đồng thời là hackintosh nên 1 số phần cứng như pin , card âm thanh , đồ hoạ v.v có thể không hoạt động nên ta phải patch DSDT/SSDT để máy hoạt động giống Mac thật nhất có thể.

Các cách patch DSDT chủ yếu:

> Có 2 cách patch dsdt chủ yếu là Static patch và Hot patch.
>
> > * Static patch đây là cách patch truyền thống đối với cách patch này ta cần dump DSDT ra và fix lỗi trực tiếp trên DSDT, sau đó bỏ nó vào bộ nạp khởi động nó sẽ được inject khi vào OS, cần gì fix đó, còn nhược điểm của cách patch này là khi update firmware ta sẽ phải patch lại toàn bộ.
> > * Còn hot patch là cách patch tìm những đoạn mã cần sửa trong DSDT và sửa lại, không sửa trực tiếp đối nó sẽ tự động inject ra khi khởi động vào OS, với cách patch này khi update firmware ta không cần patch lại.
>
> Xem chi tiết hơn ở [advanced guide](https://app.gitbook.com/o/B8HM8XRPfWObZ0DhTF5r/s/WaDTVx2hJ0rjBEHrlRj9/)
{% endhint %}

## Các tool hackintosh cơ bản

{% hint style="info" %}
Hãy cú ý thật kỹ tên của phần này "các tool hackintosh cơ bản"

> Do có rất nhiều tool cần cho việc hackintosh và hoàn thiện nên ở đây mình sẽ không liệt kê ra tất cả mà chỉ liệt kê các tool cơ bản nhất và bắt buộc phải có nhưng tool khác mình sẽ giới thiệu ở các bài sau
{% endhint %}

{% hint style="success" %}
* [SSDTTime](https://github.com/corpnewt/SSDTTime): Công cụ đơn giản nhằm tạo các SSDT cần thiết nhanh chóng, đảm bảo hoạt động dựa trên chính DSDT của máy bạn.
* [gibMacOS](https://github.com/corpnewt/gibMacOS): Tải xuống các bộ cài macOS trực tiếp từ Apple.
* [ProperTree](https://github.com/corpnewt/ProperTree): Trình chỉnh sửa .plist với các tính năng snapshot phục vụ việc load SSDTs, kexts, tools, drivers nhanh chóng.
* [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS): Tạo mới SMBIOS và áp dụng luôn vào trong config.plist.
{% endhint %}

<details>

<summary>Build app Propertree ở MacOS</summary>

* B1: Tải [ProperTree](https://github.com/corpnewt/ProperTree)
* B2: Giải nén và mở folder `Scripts`.
* B3: Mở file ​​`buildapp-select.command`

[![](https://camo.githubusercontent.com/69d381c3f92bba5bd9043eec975defb4de3c566388390fdd1c3d65a94427481e/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f377567313468485f6a41423249564d5234336e374b50574a566f685847553632484632726a4e724158787650496f474a70417234506145616f7847746f687458676134346f714937517a716339735054797973397559734b654c654d674c4859486f4345514948384e506330416d657135696a36654d76563957746753496a744b70564e6b7646373d7330)](https://camo.githubusercontent.com/69d381c3f92bba5bd9043eec975defb4de3c566388390fdd1c3d65a94427481e/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f377567313468485f6a41423249564d5234336e374b50574a566f685847553632484632726a4e724158787650496f474a70417234506145616f7847746f687458676134346f714937517a716339735054797973397559734b654c654d674c4859486f4345514948384e506330416d657135696a36654d76563957746753496a744b70564e6b7646373d7330)

* B4: Chọn phiên bản python
  * Tuỳ theo sở thích mà các bạn chọn phiên bản phù hợp&#x20;
    * Nhưng ở đây mình khuyến khích các bạn nên chọn bản đầu tiên.&#x20;
  * Nếu như các bạn chưa cài đặt python thì sript sẽ cài cho bạn việc của bạn chỉ là xác nhận muốn cài hay không bằng cách nhập `y` hoặc `n`
* B5: Các bạn ra ngoài folder sẽ thấy có app `ProperTree`  chỉ cần kéo nó vào `applications` để sử dụng

[![](https://camo.githubusercontent.com/cf1d910d585155aa6587d3ef23c9e0da397ca0101ccaa809ac454d5a3d88f689/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f6d47456356337a6172445f7a7469585a32315a474c6559567965646a524c6b4451334a7955646136543632585a34535f6750374c6936735146585f374733796b7a4f6d53704d426e755a532d7a3134713859416463455831545843384463455f6a347750536e564c41774c72626d7567686a6873366b6e2d4567436f427454704f7139346a6363313d7330)](https://camo.githubusercontent.com/cf1d910d585155aa6587d3ef23c9e0da397ca0101ccaa809ac454d5a3d88f689/68747470733a2f2f6c68332e676f6f676c6575736572636f6e74656e742e636f6d2f6d47456356337a6172445f7a7469585a32315a474c6559567965646a524c6b4451334a7955646136543632585a34535f6750374c6936735146585f374733796b7a4f6d53704d426e755a532d7a3134713859416463455831545843384463455f6a347750536e564c41774c72626d7567686a6873366b6e2d4567436f427454704f7139346a6363313d7330)

[![](https://camo.githubusercontent.com/d30a1787dbf3cfe98912ab888e9b9f87fdf069a63f4a0140aa7ddc8326cbd40b/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f376f55366c333563754a525449547a64674f707839704941597367316c59586e41323544345a7a7038344154616f765a7832306f733650596874376e5a655f47474538444d44544143595573666d6b424e3874416a6a32386a4b64483648372d6752723363506d6c61474f6f4d75363365346c6279307457426a7644687755784a687a7839594b713d7330)](https://camo.githubusercontent.com/d30a1787dbf3cfe98912ab888e9b9f87fdf069a63f4a0140aa7ddc8326cbd40b/68747470733a2f2f6c68352e676f6f676c6575736572636f6e74656e742e636f6d2f376f55366c333563754a525449547a64674f707839704941597367316c59586e41323544345a7a7038344154616f765a7832306f733650596874376e5a655f47474538444d44544143595573666d6b424e3874416a6a32386a4b64483648372d6752723363506d6c61474f6f4d75363365346c6279307457426a7644687755784a687a7839594b713d7330)

</details>

<details>

<summary>Hướng dẫn tải và sử dụng các công cụ dành cho người mới</summary>

B1: Truy cập vào link tải tool cần dùng

B2: nhấn vào `Code --> Download zip`

![](<../.gitbook/assets/image (1).png>)

B3: Giải nén và truy cập vào folder của tool

B4: Chạy tool lên thôi

* Đối với Windows thì chạy file có đuôi`.bat`
* Đối với MacOS thì chạy file có đuôi `.command`
* Đối với Linux thì chạy file `.py`

![](<../.gitbook/assets/image (2).png>)

B5: Bắt đầu sử dụng

</details>

## Một vài lưu ý

{% hint style="info" %}
Mọi ứng dụng bên MacOS đều cần cho phép và cấp quyền

> Để bỏ qua phần cho phé bạn cần disable gatekeeper xem chi tiết `tại đây`
{% endhint %}

{% hint style="info" %}
Và bạn không thực sự cần shutdown nếu như máy  bạn có thể sleep
{% endhint %}

{% hint style="info" %}
Spotlight là một công cụ tìm kiếm mạnh và tiện dụng bên MacOS
{% endhint %}

{% hint style="info" %}
Trái với suy nghĩ của nhiều người thì người dùng macos vẫn crack app như bên windows

> Một số trang crack app uy tính
>
> * [Maclife](https://maclife.io/)&#x20;
>   * Đây là trang crack app uy tính do người việt tạo ra&#x20;
>   * Trang này sử dụng Fshare để tải app khá bất tiện vì vậy bạn có thể tham khảo trang direct download cũng của maclife [tại đây](https://maclife.me/)
> * [Appstorrent](https://appstorrent.ru/)
>   * Đầy là một trang crack app cũng rất uy tính của người nga
{% endhint %}

{% hint style="info" %}
Một số app khuyên dùng khi lần đầu sử dụng&#x20;

* [Clean My Mac](https://maclife.vn/cleanmymac-x4-cong-cu-don-dep-toi-uu-he-thong-hieu-qua-nhat.html) : Giúp xoá app tận gốc và dọn dẹp.
* [Macs Fan Control](https://maclife.vn/huong-dan-cai-dat-va-cau-hinh-macs-fan-control.html) : Điều khiển tốc độ quạt máy tính.
* [Neet-downloadmanager](https://www.neatdownloadmanager.com/index.php/en/) : Hỗ trợ download nhanh.
* [EVKey](https://evkeyvn.com/) : Hỗ trợ gõ tiếng việt.&#x20;
* [iStat Menus](https://maclife.vn/istat-menus-theo-doi-thong-tren-menubar.html) : Hỗ trợ theo dõi các chỉ số cpu, fan, nhiệt độ.&#x20;
* [Archiver](https://maclife.vn/archiver-phan-mem-nen-giai-nen-nho-gon-manh-me.html) : Công cụ giải nén mạnh mẽ trên Mac.
* [Parallels Desktop](https://maclife.vn/parallels-desktop-16-for-mac-ho-tro-cai-windows-tren-mac-ban-moi-nhat.html) :  Công cụ ảo hoá mạnh mẽ.
* [Swish](https://maclife.vn/swish-ho-tro-them-rat-nhieu-thao-tac-tren-trackpad.html) : Hỗ trợ nhiều thao tác cho Trackpad.&#x20;
* [Alfred](https://maclife.io/alfred-powerpack-5-cong-cu-tuyet-voi-thay-the-cho-spotlight.html): App cài tiên của spotlight là một trong những app tuyệt vời nhất của MacOS
{% endhint %}

Source tham khảo: [https://lzhoang2601.github.io/](https://lzhoang2601.github.io/)
