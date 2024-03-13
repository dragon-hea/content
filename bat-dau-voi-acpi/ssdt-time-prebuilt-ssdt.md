# SSDT-Time Prebuilt SSDT

{% hint style="info" %}
Phần này mình dành cho các bạn newbie, bạn nào biết rồi thì có thể bỏ qua nha.
{% endhint %}

B1: Tải [SSDT-Time](https://github.com/corpnewt/SSDTTime).

B2: Mở `SSDT-Time` lên&#x20;

![](https://lh5.googleusercontent.com/Q0SSC3nTTMN\_8SjQMQmAnn4vrYK-kSQDE3M9HI6v6N9Lu2abv5y3ex--5GczffBD-ZOskBZDMJY9kiNuSeI12qwx8Nz8fbMuqjxS\_yyf-OzLkq85d9w6ZE2KJHid8GOmO12Trkw8=s0)

> Nếu các bạn gặp như hình thì chờ một tí nhé&#x20;
>
> > Nếu các bạn chờ hoài vẫn không xong thì tải iasl theo nguồn [này ](https://bitbucket.org/RehabMan/acpica/downloads/)bỏ vào mục `SSDT-Time-master ⇒ Scripts` sau đó chạy lại SSDT-Time

B3: Chọn `D` và kéo DSDT vào sau đó ấn Enter&#x20;

> Nếu chưa có DSDT thì các bạn Dump theo hướng dẫn [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/acpi-advance/patch-dsdt-phan-1)

![](https://lh4.googleusercontent.com/wWAXc4iGlDU1P5r7wMG5eDNQOnTMuNmDEZX6jjrumTT5RJAhjUuwMIFTehBLGBY8QLVsMnYhIrVltAfsYe-rH8nEghZGGLixMb2yGP1LmT6vcQx8EmUGljKaJ9LzQ49gUykjcsk2=s0)

B4: Chọn các ssdt cần `Prebuilt`&#x20;

> VD 1,2,3&#x20;
>
> 1 vài SSDT không cần nó sẽ báo no need

![](https://lh5.googleusercontent.com/0fOw4vX7tICimfRGPZubP0Z70W2X7UC00W1AhKqDlyy2dVHGAdtpA3oIWB-XZ7obVw5-PwQi6uCSYXBAp0SMzHODtAH3FPGB3jZZVLdCjjy-RNEkKVJdKqUdDnDEhExdO-wTjRR3=s0)

B5: Sau đó bỏ nó vào `EFI ⇒ OC ⇒ ACPI` tiếp các bạn `Snapshot`.

> Hoặc `EFI ⇒ Clover ⇒ ACPI ⇒ Patched`

B6: Reboot và tận hưởng thôi.&#x20;

> Lưu ý : Chỉ Build những SSDT nào thật sự cần thiết nếu không rất dễ Panic.
