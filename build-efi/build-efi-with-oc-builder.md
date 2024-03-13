# Build EFI with OC Builder

{% hint style="info" %}
Nếu các bạn cảm thấy việc build EFI bằng tay quá rườm ra thì có thể đọc guide này. Rất cảm ơn bác lzhoang2601 đã build ra tool OC-Builder.

Nhìn chung tool này build EFI khá chuẩn.&#x20;

> Chuẩn hơn nhiều so với newbie buid, cũng như một số bác chưa có kinh nghiệm nhiều

Để bắt đầu guide này mình xin chân thành cảm ơn bác [lzhoang2601](https://www.facebook.com/hutieu2804)
{% endhint %}

B1: Các bạn truy cập trang web [sau](https://lzhoang2601.github.io/)

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

B2: Các bạn tải `aida64` [tại đây](https://hackintosher-my.sharepoint.com/:u:/g/personal/hoanglongvonguyen\_hackintosher\_onmicrosoft\_com/EaKBpVF52WxHvQASAGrjpz0B2CRvUQh\_pAzZryZ9X6ggVA?e=ojmr7Q)

> Pass: heavietnam

B3: Mở aida64 và truy cập đường dẫn sau `Motherboard ==> Motherboard ==> Motherboard Name` và điền `value` nhận được vào mục `Mainboard name. Mini PC / Laptop model` của tool

<figure><img src="../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

B2: Tiếp tực truy cập vào đường dẫn `Motherboard ==> CPU ==> CPU Type`  điền `vaule`  vừa nhận được vào mục `CPU NAME` của tool

> Chỉ điền mã chip và thế hệ chip
>
> > Vd i3-1115G4

B3: Các bạn truy cập vào `Display ==> GPU ==> GPU Code Name` điền vaule vừa nhận được vào mục `dGPU Name`

> Do ở đây mình không có DGPU nên mình bỏ qua mục này nếu các bạn có thì cứ làm như trên

B4: Các bạn truy cập vào `Network ==> PCI / PnP Network ==> Device Description`  điền `vaule`  vừa nhận được vào `WiFi Card Name` hoặc `Ethernet Name`

<figure><img src="../.gitbook/assets/image (83).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

B5: Các bạn nhấn tổ hợp `Windows + i` mở `Network & internet ==> Properties ==> Physical address (MAC)` điền giá trị vừa nhận được vào mục `MAC Address`

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

B6: Tiếp tục mở aida64 và truy cập `Multimedia ==> PCI / PnP Audio ==> Device Description`

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

B7: Các bạn truy cập tiếp đường dẫn `Storage ==> Storage ==> Drive` các bạn để ý tên ổ cứng nếu hiện `Nvme` thì chọn `Storage Controller` là `NVME`. Nếu hiện là ATA thì tức là Sata chọn `Storage Controller` là `SATA`&#x20;

> Tuy nhiên vẫn có một số ổ không hiện gì cả. Nếu như thế thì các bạn copy tên ổ cứng search sẽ ra chuẩn kết nối của ổ cứng đó cũng chỉnh là chuẩn kết nối của `Storage Controller`

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

B8: Chọn phiên bản MacOs phù hợp với bạn ở mục `macOS Version`

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

B9: Chọn phiên bản OpenCore mà bạn thích ở mục `OpenCore Version`&#x20;

* Acidanthera: Chính là bản `OpenCorePkg RELEASE`
* Btwise: tức là `OpenCorePkg No ACPI` xem chi tiết vể version này [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/efi-opencore-no-acpi)

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

> Như vậy cơ bản là xong rồi sau khi hoàn thành nó sẽ có dạng

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

B10: Ấn nút `Create EFI`

B11: Ấn `Downloads`

{% hint style="info" %}
Một số phần cứng khi các bạn điền vào nó có đề xuất thì các bạn hãy chọn phần đề xuất thì sẽ đỡ tốn công. Đặc biệt là khi bàn điền mục mainboard
{% endhint %}

> Và giờ thì tận hưởng thôi :smile:
