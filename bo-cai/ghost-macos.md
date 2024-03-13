# Ghost macos

> Nghe có vẻ lạ nhưng đây là 1 chuyện hoàn toàn có thể thực hiện được. Nó hoàn toàn giống như ghost win tạo ra một bản macos của riêng bạn và khi bung chỉ mất vài phục thật tuyệt vời đúng không nào. Còn chờ gì nữa mà không bắt đầu ngay thôi ! :smile:

## Restore

> Tương tự như trên ta cũng cần 1 môi trường windows và app R-Drive Image. đương nhiên cũng không thể thiếu 1 file image định dạng rdr

B1: Download image macos [tại đây](image.md)

B2: tạo một phân vùng để restore image định dạng là NTFS

B3: chọn Restore image

![](https://i.imgur.com/xczIUdr.png)

B4: chọn file image định dạng rdr đã tải ở trên

![](https://i.imgur.com/PoiJ26b.png)

B5: chọn phân vùng cần restore và phân vùng muốn restore lên

![](https://i.imgur.com/9UcKTOU.png)

B6: ấn next và ấn start

![](https://i.imgur.com/gKKvePc.png)

và như vậy là done rồi

> Nếu các bạn cài trong ổ cứng riêng thì chỉ việc mount phân vùng efi đã restore ra rồi build efi theo hướng dẫn [tại đây](../build-efi/creat-efi.md)

> Nếu như các bạn cài đặt chung với windows trên một ổ cứng thì có 2 cách
>
> * Tạo 1 cái phân vùng 200mb định dạng Fat32 bằng [minitool](https://www.partitionwizard.com/)
>   * Sau đó thêm [EFI](../build-efi/creat-efi.md) vào phân vùng này rồi add boot bằng easy uefi chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/multiboot/cach-them-boot-vao-bios)
> * Nếu bạn dual windows trên 1 ổ cứng thì khi restore các bạn không restore partion EFI mà resize partion theo hướng dẫn [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/multiboot/resize-va-create-partion-efi)
>   * Sau đó mount partion `EFI` ra rồi thêm efi vào chung folder [`EFI` ](../build-efi/creat-efi.md)với folder `Microsoft`

> Các bạn định cài macos thì chỉ việc đọc phần này thôi. Bạn nào có nhu cầu có thể tham khảo thêm phần [ghost](ghost-macos.md#ghost)

## Ghost

B1: Chuẩn bị 1 môi trường windows

> Có thể là winpe hoặc là windows LTSC, Ghost,…

B2: tải app R-Drive Image [tại đây](https://www.drive-image.com/)

B3: Chọn `Create image`

![](https://i.imgur.com/jypsi7u.png)

B4: chọn phân vùng cần ghost

![](https://i.imgur.com/fuAVAYW.png)

B6: ấn next và chọn nơi lưu image

![](https://i.imgur.com/5ptN92o.png)

B7: check lại thông tin phân vùng rồi ấn Start

![](https://i.imgur.com/O66aRmV.png)

Và như vậy là done
