# Tìm hiểu chung về phần cứng

## CPU

{% hint style="danger" %}
Hãy lưu ý rằng ở cột Last supported version mình ghi current 10.5.8 có nghĩa là CPU này có hỗ trợ ở version đó chứ không đồng nghĩa là IGPU cũng được hỗ trợ.
{% endhint %}

<details>

<summary>Một số yếu tố chung cần biết</summary>

* Mọi CPU Intel từ đời Yonah đều được hỗ trợ.

<!---->

* CPU 32bit được hỗ trợ từ 10.4.1 --> 10.6.8

<!---->

* CPU 64bit được hỗ trợ từ 10.4.1+

<!---->

* SEE hỗ trợ:
  * SSE3 : hỗ trợ tất cả các version OS X/macOS.
  * SSSE3: hỗ trợ tất cả các version 64 bit OS X/macOS.
  * SSE4: hỗ trợ 10.12+
  * SSE4.2: hỗ trợ 10.14+

<!---->

* Firmware hỗ trợ:
  * 10.4.1-10.4.7 yêu cầu EFI32 (IA32)
  * 10.4.8-10.7.5 hỗ trợ cả 32 bit lẫn 64 bit.
  * 10.8+ yêu cầu EFI64 (X64).
  * 10.7-10.9 yêu cầu OpenPartitionDxe.efi để có thể boot được vào phần vùng Recovery.

<!---->

* Yêu cầu của kernel:
  * 10.4-10.5 yêu cầu kext 32-bit do chỉ support 32-bit kernelspace.
  * 10.6-10.7 support cả 32-bit và 64-bit.
  * 10.8+ yêu cầu kext 64-bit do chỉ support 64-bit kernelspace.

<!---->

* Nhân và luồng:
  * Từ 10.10 có thể không boot được với 24 luồng nó sẽ gặp lỗi `mp_cpus_call_wait() timeout` panic.
  * Từ 10.11+ bị giới hạn 64 luồng.
  * cpus=1 có thể là giải pháp giúp disable hyperthreading.

<!---->

* Lưu ý:
  * Lilu yêu cầu version 10.8+ (đối với OS X thì nên dùng FakeSMC)
  * Đối với version 10.6 và cũ hơn sẽ yêu cầu RebuildAppleMemoryMap.
  * Nhiều tính năng trên macos hoàn toàn ko hoạt động và 1 số bị lỗi trên CPU AMD bao gồm:
    * Ảo hóa dựa trên Apple HV (VMWare, Parallels, Docker, Android Studio,….) bị vô hiệu hóa VirtualBox được hỗ trợ.
    * Các phần mềm Adobe.
    * Phần mềm 32-bit.
    * Và 1 vài app audio.

</details>

### Intel Desktop và Laptop

> Bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support)
>
> > Thank you

<table data-full-width="false"><thead><tr><th width="151">CPU Generation</th><th width="151">Initial support</th><th width="144">Last supported version</th><th width="144">Notes</th><th>CPUID</th></tr></thead><tbody><tr><td>Pentium 4</td><td>10.4.1</td><td>10.5.8</td><td>chỉ sử dụng trong dev kits</td><td>0x0F41</td></tr><tr><td>Yonah</td><td>10.4.4</td><td>10.6.8</td><td>32-Bit</td><td>0x0006E6</td></tr><tr><td>Conroe, Merom</td><td>10.4.7</td><td>10.11.6</td><td>No SSE4</td><td>0x0006F2</td></tr><tr><td>Penryn</td><td>10.4.10</td><td>10.13.6</td><td>No SSE4.2</td><td>0x010676</td></tr><tr><td>Nehalem</td><td>10.5.6</td><td>Current</td><td>N/A</td><td>0x0106A2</td></tr><tr><td>Lynnfield, Clarksfield</td><td>10.6.3</td><td>Current</td><td>N/A</td><td>0x0106E0</td></tr><tr><td>Westmere, Clarkdale, Arrandale</td><td>10.6.4</td><td>Current</td><td>N/A</td><td>0x0206C0</td></tr><tr><td>Sandy Bridge</td><td>10.6.7</td><td>Current</td><td>N/A</td><td>0x0206A0(M/H)</td></tr><tr><td>Ivy Bridge</td><td>10.7.3</td><td>Current</td><td>N/A</td><td>0x0306A0(M/H/G)</td></tr><tr><td>Haswell</td><td>10.8.5</td><td>Current</td><td>N/A</td><td>0x0306C0(S)</td></tr><tr><td>Broadwell</td><td>10.10.0</td><td>Current</td><td>N/A</td><td>0x0306D4(U/Y)</td></tr><tr><td>Skylake</td><td>10.11.0</td><td>Current</td><td>N/A</td><td>0x0506e3(H/S) 0x0406E3(U/Y)</td></tr><tr><td>Kaby Lake</td><td>10.12.4</td><td>Current</td><td>N/A</td><td>0x0906E9(H/S/G) 0x0806E9(U/Y)</td></tr><tr><td>Coffee Lake</td><td>10.12.6</td><td>Current</td><td>N/A</td><td>0x0906EA(S/H/E) 0x0806EA(U)</td></tr><tr><td>Amber, Whiskey, Comet Lake</td><td>10.14.1</td><td>Current</td><td>N/A</td><td>0x0806E0(U/Y)</td></tr><tr><td>Comet Lake</td><td>10.15.4</td><td>Current</td><td>N/A</td><td>0x0906E0(S/H)</td></tr><tr><td>Ice Lake</td><td>10.15.4</td><td>Current</td><td>N/A</td><td>0x0706E5(U)</td></tr><tr><td>Rocket Lake</td><td>10.15.4</td><td>Current</td><td>Yêu cầu Comet Lake CPUID</td><td>0x0A0671</td></tr><tr><td>Tiger Lake</td><td>10.15.4</td><td>Current</td><td>Yêu cầu Comet Lake CPUID</td><td>0x0806C0(U)</td></tr><tr><td>Alder Lake</td><td>10.15.4</td><td>Current</td><td>Yêu cầu Comet Lake CPUID</td><td>N/A</td></tr><tr><td>Raptor lake</td><td>10.15.4</td><td>Current</td><td>Yêu cầu Comet Lake CPUID</td><td>N/A</td></tr></tbody></table>

### Intel High-End Desktop và Server CPUs được hỗ trợ <a href="#intel-high-end-desktop-va-server-cpus-duoc-ho-tro" id="intel-high-end-desktop-va-server-cpus-duoc-ho-tro"></a>

{% hint style="danger" %}
Những CPU thuộc nhóm này không dành cho newbie vì nó rất khó cài
{% endhint %}

<table><thead><tr><th width="334.3333333333333">CPU Generation</th><th>Initial support</th><th>Last supported version</th></tr></thead><tbody><tr><td>Nehalem, Westmere</td><td>OS X 10.5.6, Leopard</td><td>macOS 12 Monterey</td></tr><tr><td>Sandy and Ivy Bridge-E</td><td>OS X 10.9, Mavericks</td><td>macOS 12 Monterey</td></tr><tr><td>Haswell-E</td><td>OS X 10.11, El Capitan</td><td>Current</td></tr><tr><td>Broadwell-E</td><td>OS X 10.11, El Capitan</td><td>Current</td></tr><tr><td>Skylake-X/W and Cascade Lake-X/W</td><td>macOS 10.13, High Sierra</td><td>Current</td></tr></tbody></table>

### AMD CPUs <a href="#amd-cpus" id="amd-cpus"></a>

{% hint style="warning" %}
Đối với APU AMD thì sử dụng nooted red bạn nhé xem chi tiết [tại đây](https://heavietnam.gitbook.io/untitled/bat-dau-voi-acpi/tim-hieu-ve-kext#graphics)
{% endhint %}

<table><thead><tr><th width="188.33333333333331">CPU Generation</th><th width="169">Initial support</th><th width="150">Last supported version</th><th>Note</th></tr></thead><tbody><tr><td>Bulldozer(15h) -Jaguar(16h)</td><td>macOS 10.13, High Sierra</td><td>macOS 12 Monterey</td><td>không support cho laptop</td></tr><tr><td>Ryzen</td><td>macOS 10.13, High Sierra</td><td>Current</td><td><p>Có support cho laptop:</p><ul><li><code>1xxx</code> to <code>5xxx</code> series</li><li><code>7x30</code> series</li><li>Từ MacOS 11+</li></ul></td></tr><tr><td>Threadripper(17h and 19h)</td><td>macOS 10.13, High Sierra</td><td>Current</td><td>không support cho laptop</td></tr></tbody></table>

## GPU

{% hint style="info" %}
Xem chi tiết [tại đây](https://app.gitbook.com/o/B8HM8XRPfWObZ0DhTF5r/s/eTy458FK3H2iOZTEsqFz/)
{% endhint %}

<details>

<summary>Các yếu tố chung cần biết</summary>

* Các GPU AMD nhân GCN được hỗ trợ trên các phiên bản mới nhất.
* Các APU AMD không được hỗ trợ.
* Các GPU AMD nhân Lexa không được hỗ trợ chính thức.
* GeForce 900 series (Maxwell 9XX), GeForce 10 series (Pascal 10XX) được hỗ trợ giới hạn đến macOS 10.13 (High Sierra)
* GeForce 20 series, GeForce 16 series không được hỗ trợ.
* GeForce 30 series cũng không được hỗ trợ.
* GeForce 600 series, GeForce 700 series (Kepler) vẫn đang được hỗ trợ hiện tại là Monterey beta 9&#x20;
  * Từ Monterey phải dùng tool `Geforce-Kepler-patcher` để patch không native, xem hướng dẫn [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/patch-card-do-hoa-nvidia)
* iGPU GT2 được hỗ trợ.
* GMA series được hỗ trợ 1 số dòng, xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/patch-gma-gpu).
* iGPU GT1 trên pentium, Celerons và Atoms không được hỗ trợ.

</details>

### Intel

<details>

<summary>Các yếu tố chung cần biết</summary>

* HD Graphics của CPU Celeron, Pentium, Atom đều không dùng được, PC thì phải có thêm card rời
* Hiện tại iGPU của dòng Tiger Lake (gen 11)+ không dùng được
* Desktop: HD 2500 tạch, HD 4000 trở đi dùng được
* Laptop: HD 3000 tối đa 10.13.6, HD 4000 tối đa 11.6, HD 4XXX-HD 5xx trở về sau có thể cài bản tối đa là monterey
  * Tuy nhiên Đối với HD-5xx bạn có thể fake device-id thành kabylake để chạy trên ventura xem chi tiết [tại đây](../config-laptop/skylake.md)

</details>

> Bảng tóm tắt các version hỗ trợ cho các đời iGPU Intel (bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support)
>
> > Thank you

<table><thead><tr><th>GPU Generation</th><th width="151">Initial support</th><th>Last supported version</th><th>Notes</th></tr></thead><tbody><tr><td>3rd Gen GMA</td><td>10.4.1</td><td>10.7.5</td><td>Yêu cần 32 bit và kernel patch xem chi tiết <a href="https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/patch-gma-gpu">tại đây</a></td></tr><tr><td>4th Gen GMA</td><td>10.5.0</td><td>10.7.5</td><td>Yêu cần 32 bit và kernel patch xem chi tiết <a href="https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/patch-gma-gpu">tại đây</a></td></tr><tr><td>Arrandale(HD Graphics)</td><td>10.6.4</td><td>10.13.6</td><td>Chỉ support LVDS , eDP và external output không support</td></tr><tr><td>Sandy Bridge</td><td>10.6.7</td><td>N/A</td><td>N/A</td></tr><tr><td>Ivy Bridge</td><td>10.7.3</td><td>11.7.x</td><td>N/A</td></tr><tr><td>Haswell</td><td>10.8.5</td><td>12.6.x</td><td>N/A</td></tr><tr><td>Broadwell</td><td>10.10.0</td><td>12.6.x</td><td>N/A</td></tr><tr><td>Skylake</td><td>10.11.0</td><td>12.6.x</td><td>Vẫn có thể fake <code>device-id</code> để chạy trên ventura</td></tr><tr><td>Kaby Lake</td><td>10.12.4</td><td>Current</td><td>N/A</td></tr><tr><td>Coffee Lake</td><td>10.13.6</td><td>Current</td><td>N/A</td></tr><tr><td>Comet Lake</td><td>10.15.4</td><td>Current</td><td>N/A</td></tr><tr><td>Ice Lake</td><td>10.15.4</td><td>Current</td><td>Yêu cầu <code>-igfxcdc</code> và <code>-igfxdvmt</code>  trong boot-arg</td></tr><tr><td>Tiger Lake</td><td>N/A</td><td>N/A</td><td>không khả dụng</td></tr><tr><td>Rocket Lake</td><td>N/A</td><td>N/A</td><td>không khả dụng</td></tr><tr><td>Alder Lake</td><td>N/A</td><td>N/A</td><td>không khả dụng</td></tr><tr><td>Raptor lake</td><td>N/A</td><td>N/A</td><td>không khả dụng</td></tr></tbody></table>

### AMD

<details>

<summary>Các yếu tố chung cần biết</summary>

* AMD APUs Ryzen được hỗ trợ một vài dòng
  * `1xxx` to `5xxx` series
  * `7x30` series
  * Từ MacOS 11+
* Dòng AMD Lexa không dùng được
  * Có thể Fake device-id để fix nó xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/patch-card-do-hoa-amd)
* Dòng AMD Polaris trở về sau dùng tốt: RX 4XX, RX 5XX, Vega 56/64, RX 5X00, RX 6600, RX 6600XT, RX 6800XT, RX 6900XT,..
* Còn RX 6500XT, RX 6700XT không dùng được
* Dòng AMD cũ hơn có loại thì native có loại thì không dùng được, nhiều loại phải Fake GPU ID mới dùng được
  * Xem chi tiết cách này [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/patch-card-do-hoa-amd)

</details>

> Bảng tóm tắt các phiên bản macOS hỗ trợ cho các đời GPU AMD (bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support)&#x20;
>
> > Thank you

<table><thead><tr><th width="177">GPU Generation</th><th width="170">Initial support</th><th width="156">Last supported version</th><th>Notes</th></tr></thead><tbody><tr><td>X800</td><td>10.3.x</td><td>10.7.5</td><td>Yêu cầu 32 bit kernel</td></tr><tr><td>X1000</td><td>10.4.x</td><td>10.7.5</td><td>N/A</td></tr><tr><td>TeraScale</td><td>10.4.x</td><td>10.13.6</td><td>N/A</td></tr><tr><td>TeraScale 2/3</td><td>10.6.x</td><td>10.13.6</td><td>N/A</td></tr><tr><td>GCN 1</td><td>10.8.3</td><td>12.6.x</td><td>N/A</td></tr><tr><td>GCN 2/3</td><td>10.10.x</td><td>12.6.x</td><td>N/A</td></tr><tr><td>Polaris 10, 20</td><td>10.12.1</td><td>Current</td><td>N/A</td></tr><tr><td>Vega 10</td><td>10.12.6</td><td>Current</td><td>N/A</td></tr><tr><td>Vega 20</td><td>10.14.5</td><td>Current</td><td>N/A</td></tr><tr><td>Navi 10</td><td>10.15.1</td><td>Current</td><td>Yêu cầu <code>agdpmod=pikera</code>  trong boot-arg</td></tr><tr><td>Navi 20</td><td>11.4</td><td>Current</td><td>Hiện tại chỉ 1 vài Navi 21 models còn support</td></tr><tr><td>Navi 21</td><td>MacOS BigSur</td><td>Current</td><td>Yêu cầu <code>agdpmod=pikera</code>  trong boot-arg</td></tr><tr><td>Navi 23</td><td>MacOS Monterey</td><td>Current</td><td>Yêu cầu <code>agdpmod=pikera</code>  trong boot-arg</td></tr></tbody></table>

### NVDIA

<details>

<summary>Yếu tố chung cần biết</summary>

* Dòng Kelper (6XX, 7XX) native cho tới macOS Big Sur
  * Muốn dùng cho macOS Monterey phải cần chạy tool của [chirs111](https://github.com/chris1111/Geforce-Kepler-patcher)
* Dòng Maxwell, Pascal (9XX, 10XX) cần webdriver mới chạy nhưng chỉ hỗ trợ tối đa bản High Sierra 10.13.6
* Dòng Turing (16XX, 20XX), Ampere (30XX) không hỗ trợ
* Dòng Fermi trở về trước có thể cài Sierra 10.12 trở về trước nhưng đã cũ quá rồi nên bỏ đi

</details>

> Bảng tóm tắt các version hỗ trợ cho các đời dGPU Intel (bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support)&#x20;
>
> > Thank you

<table><thead><tr><th>GPU Generation</th><th width="154">Initial support</th><th width="153">Last supported version</th><th>Notes</th></tr></thead><tbody><tr><td><a href="https://en.wikipedia.org/wiki/GeForce_6_series">GeForce 6</a></td><td>10.2.x</td><td>10.7.5</td><td>Yêu cầu 32 bit kernel và <a href="https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector">NVCAP patching</a></td></tr><tr><td><a href="https://en.wikipedia.org/wiki/GeForce_7_series">GeForce 7</a></td><td>10.4.x</td><td>10.7.5</td><td>Yêu cầu <a href="https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/connector/fix-connector"> NVCAP patching</a></td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Tesla_(microarchitecture)">Tesla</a></td><td>10.4.x</td><td>10.13.6</td><td>N/A</td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Tesla_(microarchitecture)#Tesla_2.0">Tesla v2</a></td><td>10.5.x</td><td>10.13.6</td><td>N/A</td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Fermi_(microarchitecture)">Fermi</a></td><td>10.7.x</td><td>10.13.6</td><td>N/A</td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Kepler_(microarchitecture)">Kepler</a></td><td>10.7.x</td><td>11.7.x</td><td>Có thể chạy trên Monterey bằng việc chạy tool có nói ở trên</td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Kepler_(microarchitecture)">Kepler v2</a></td><td>10.8.x</td><td>11.7.x</td><td>Có thể chạy trên Monterey bằng việc chạy tool có nói ở trên</td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Maxwell_(microarchitecture)">Maxwell</a></td><td>10.10.x</td><td>10.13.6</td><td>Yêu cầu<a href="https://www.nvidia.com/download/driverResults.aspx/149652/"> NVIDIA Web Drivers</a></td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Pascal_(microarchitecture)">Pascal</a></td><td>10.12.4</td><td>10.13.6</td><td>N/A</td></tr><tr><td><a href="https://en.wikipedia.org/wiki/Turing_(microarchitecture)">Turing</a></td><td>N/A</td><td>N/A</td><td>không khả dụng</td></tr></tbody></table>

## Mainboard

{% hint style="danger" %}
Lưu ý mọi từ `Vga` được dùng trong bài đều có nghĩa là một công xuất hình thay vì card rời

> Đây là một sự nhầm lẫn rất tai hại của nhiều người
{% endhint %}

### Intel[​](https://vnohackintosh.com/docs/basic-knowledge/limits/#intel-1) <a href="#intel-1" id="intel-1"></a>

* Đa số main dòng B, H, Z đều được hỗ trợ.
* Main dòng X hay main server thì khó cài đặt, có thể sẽ phải mod bios!
* Nếu xác định không dùng card rời thì mainboard phải có cổng DVI, HDMI, DP
  * &#x20;Hoặc Type C có hỗ trợ DP hoặc Thunderbolt
* Khi dùng CPU Intel đời Skylake hoặc mới hơn, cổng VGA được xem như là DP khi sử dụng macOS.
* Với mainboard 500 series mặc dù có thể sử dụng CPU Comet Lake nhưng không thể kích hoạt iGPU để xuất hình mà chỉ dành phục vụ cho `Intel Quick Sync`.

### AMD <a href="#amd-1" id="amd-1"></a>

{% hint style="info" %}
Đa số Mainboard AMD đều được hỗ trợ tuy nhiên vẫn còn nhiều hạn chế
{% endhint %}

## Ổ cứng

{% hint style="danger" %}
Phần này có tham khảo từ nhiều nguồn:

* [https://lzhoang2601.github.io/](https://lzhoang2601.github.io/)
* [https://vnohackintosh.com/](https://vnohackintosh.com/)
* [https://github.com/dortania/bugtracker/issues/192](https://github.com/dortania/bugtracker/issues/192)
{% endhint %}

{% hint style="info" %}
SSD chất lượng tốt không chỉ là yêu cầu cần thiết dành cho macOS mà còn với cả Windows cùng các hệ điều hành khác để đảm bảo trải nghiệm và dữ liệu của bạn.
{% endhint %}

Phần lớn các ổ cứng đều được hỗ trợ ngoại trừ một số dòng sau đây:

* Các ổ Samsung PM981, PM991 and Micron 2200S NVMe SSDs hỗ trợ không tốt do đó bạn cần [NVMeFix.kext](https://github.com/acidanthera/NVMeFix/releases) trong `EFI ==> OC ==> Kex`t và snaps&#x20;
  * Hoặc `EFI ==> Clover ==> Kext ==> other`
* Tuy nhiên ổ Samsung 970 EVO Plus NVMe SSDs trước đó gặp 1 vài vấn đề nhưng ở bản FIRMWARE mới nhất đã được fix các bạn có thể tải [ở đây](https://www.samsung.com/semiconductor/minisite/ssd/download/tools/).
* Intel 600p hỗ trợ không tốt nó có rất nhiều bug&#x20;
  * Khuyến khích nên tránh&#x20;
  * Cần [NVMeFix](https://github.com/acidanthera/nvmefix) để khởi động
* Một sỗ mẫu SSD NVMe sẽ không tương thích với macOS gây lỗi không thể khởi động, force restart, tốc độ chậm,... SSD SATA cũng gây lỗi khi dùng macOS không riêng gì SSD NVMe.
  * Cơ bản tất cả các đĩa cứng SATA đều được hỗ trợ, nhưng nếu dùng ổ cứng chất lượng kém sẽ ảnh hưởng tới trải nghiệm sử dụng
* Tất cả các đĩa cứng eMMC đều không thể điều khiển được&#x20;
  * Phổ biến ở một số máy tính bảng hoặc máy tính xách tay cấp thấp
  * Bạn có thể sử dụng kext [EmeraldSDHC](https://github.com/acidanthera/EmeraldSDHC) để sử dụng ổ cứng chuẩn eMMC

<details>

<summary>Các ổ cứng hoạt động hoàn hảo với trim</summary>

* Western Digital Blue SN550
* Western Digital Black SN700
* Western Digital Black SN720
* Western Digital Black SN750 (bao gồm cả mã OEM SN730)
* Western Digital Black SN850
* Intel 760p (bao gồm mẫu OEM như SSDPEMKF512G8）
* KingDian S280
* Kingchuxing
* Crucial P1 1TB NVME (SM2263EN)
* KingDian S280
* PLEXTOR M5Pro (SATA)
* Samsung 850 EVO/PRO (SATA)
* Samsung 860 EVO/PRO (SATA)
* Samsung 870 EVO/EVO (SATA)

</details>

<details>

<summary>Các ổ cứng mà trim không hoạt động</summary>

* Samsung 950 Pro
* Samsung 960 Evo/Pro
* Samsung 970 Evo/Pro (Cần dùng firmware mới nhất)

</details>

<details>

<summary>Các ổ cứng không tương thích với IONVMeFamily</summary>

Nếu có sẵn những mẫu sau đôi khi bạn sẽ bị stuck nếu gặp thì sẽ cần dùng SSDT để Disable chúng đi xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/acpi-advance/disable-unsupported-nvme)

* 512 GB GIGABYTE M.2 PCIe SSD (VD GP-GSM2NE8512GNTD）
* ADATA Swordfish 2 TB M.2-2280
* SK Hynix HFS001TD9TNG-L5B0B
* SK Hynix P31
* PC601/PC611/PC711/BC501（chủ yếu được tìm thấy trong máy tính xách tay Lenovo và Dell, một số lô không thể cài đặt macOS)
* Samsung PM961/PM981/PM981a/PM991
* Samsung 983ZET
* Micron 2200V MTFDHBA512TCK
* Micron 2200S
* Intel 600P/660P/760P (với một số vấn đề lạ)
* Kingston A2000 (Cần [NVMeFix.kext](https://github.com/acidanthera/NVMeFix)）
* Asgard AN3+ (STAR1000P)
* Netac NVME SSD 480
* Kingmax NVME SSD

</details>

## Ethernet

{% hint style="info" %}
Xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/network/fix-ethernet)
{% endhint %}

{% hint style="danger" %}
Phần này có trích dẫn từ nguồn [https://lzhoang2601.github.io/](https://lzhoang2601.github.io/)
{% endhint %}

Hầu hết đều được hỗ trợ tốt

* Các phần cứng được hỗ trợ như:
  * `Qualcomm`: Atheros AR816x, AR817x, Killer E220x, Killer E2400 và Killer E2500(dựa trên Realtek RTL8111).
  * `Realtek`: RTL8111, RTL8100, RTL8125, Killer E2600, Killer E3000 và các phần cứng cũ hơn dựa vào 10/100MBe.
  * `Intel`: 82578, 82579, I211, I217, I218, I219, I255-V, I350 và các phần cứng cũ hơn dựa vào 10/100MBe.
* Cùng nhiều phần cứng của Aquantia, Broadcom, Intel,... được hỗ trợ sẵn trong macOS bởi được sử dụng trên các máy Mac.

## WiFi và Bluetooth

{% hint style="info" %}
Xem chi tiết về card wifi và bluetooth [tại đây](https://app.gitbook.com/o/B8HM8XRPfWObZ0DhTF5r/s/t9lszUqAVtiIeey6nrdS/)

Xem các patch chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/network/fix-wifi-va-bluetooth)&#x20;
{% endhint %}

{% hint style="danger" %}
Ở phần này có tham khảo và trích dẫn ở source&#x20;

[https://vnohackintosh.com/docs/basic-knowledge/limits/](https://vnohackintosh.com/docs/basic-knowledge/limits/)
{% endhint %}

<details>

<summary>Các yếu tố chung cần biết</summary>

* Card wifi đi kèm với hầu hết các máy tính xách tay không được hỗ trợ bởi Apple
* Card wifi tốt nhất là của Broadcom được sử dụng bởi Apple (hàng tháo máy real mac)
* Card wifi Intel đã có thể hoạt động tốt, tuy nhiên bluetooth intel vẫn đang khá là bất ổn
* Wifi Broadcom dòng BCM9452ZAE(cần kext và patch), BCM9452 (cần kext), BCM94331(cần kext từ Catalina trở đi), BCM94360 (native) hoạt động tốt, đầy đủ tính năng airdrop, handoff
* Wifi Atheros/Qualcom có sẵn trên laptop hiện nay đều tạch, có mấy mã atheros dùng được nhưng đã hết hỗ trợ ở 11.0
  * AR9565&#x20;
  * AR9462&#x20;
  * AR9463&#x20;
  * AR9485
* Có thể dùng usb wifi, usb bluetooth được hỗ trợ&#x20;
  * Giá rất rẻ

</details>

{% hint style="info" %}
Danh sách các card wifi có thể hoạt động tốt, native, dùng được tất chức năng của mac bao gồm airdrop, handoff xem chi tiết [tại đây](https://app.gitbook.com/o/B8HM8XRPfWObZ0DhTF5r/s/t9lszUqAVtiIeey6nrdS/)
{% endhint %}

## Audio

{% hint style="danger" %}
Ở phần này có tham khảo và trích dẫn ở source [https://lzhoang2601.github.io/hardware/hardware-supported](https://lzhoang2601.github.io/hardware/hardware-supported)
{% endhint %}

<details>

<summary>Một số yếu tố chung cần biết</summary>

* Combojack ( giắc cắm tai nghe kết hợp microphone ) trên đa số laptop sẽ không hoạt động được microphone rời mà chỉ hoạt động được microphone sẵn trong máy. Ngoại trừ một số codec như: ALC255, ALC256, ALC295, ALC298,... hoạt động được khi sử dụng [ComboJack](https://github.com/hackintosh-stuff/ComboJack).
* Những laptop sử dụng Intel Smart Sound Technology đều không được hỗ trợ với macOS.
* Đa số codec âm thanh đều được hỗ trợ với dự án [AppleALC](https://github.com/acidanthera/AppleALC). Xem thêm tại [Supported codecs](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs).
* Với codec ALC4080, tai nghe và microphone sử dụng kết nối USB đều được hỗ trợ mà không cần kext [AppleALC](https://github.com/acidanthera/AppleALC).

</details>

{% hint style="info" %}
Một số guide và cách patch Audio

* Patch HPET và Apple ALC
  * Xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/patch-am-thanh-voi-applealc)
  * Đây là một cách phổ biến và dễ dùng
  * Thật chất nó chính là các bản patch applehda được patch sẵn và inject thông qua các `layout-id`
* Tinh chỉnh Voodoohda
  * Xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/audio/patch-audio-voi-voodoohda)
  * Thường được dùng để cứu vớt khi không thể patch Apple HDA và Apple ALC bị thiếu một vài device
* Patch Apple HDA
  * Xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/audio/patch-apple-hda)
  * Đây là cách hoàn mĩ nhất nhưng khó hơn hai cách kia
{% endhint %}

## Bảo mật <a href="#bao-mat" id="bao-mat"></a>

{% hint style="info" %}
Hãy cân nhắc trước khi cài hackintosh

Các yếu tố sau sẽ không hoạt động

* Cảm biến vân tay
* Windows Hello Face
  * Với kết nối USB, bạn có thể sử dụng được camera nếu may mắn&#x20;
    * Microsoft Surface Laptop 3,...
  * Với kết nối I2C (thông qua iGPU) sẽ không hoạt động bất kì chức năng nào liên quan.
{% endhint %}

## Các chức năng khác

### Sleep

{% hint style="info" %}
Khá là hên xuôi và nhiều phương án để patch xem chi tiết [tại đây](https://app.gitbook.com/s/auskGAp5wYbI1xQWn4YZ/universal/fix-sleep)

Khuyến khích thử các cách sau trước tiên

* Hibernation
* Map USB
* Patch-GPRW
* Darkwake
* Force-online
{% endhint %}

### Thunderbolt USB-C

{% hint style="info" %}
Xem chi tiết [tại đây](https://app.gitbook.com/s/WaDTVx2hJ0rjBEHrlRj9/general/hotplug-thunderbolt-3)

Hầu hết đều có thể cold plug. Tức là cắm trước khi khởi động

Tuy nhiên nếu bạn muốn hot plug thì xem guide ở trên
{% endhint %}

{% hint style="danger" %}
Những hãy nhớ rằng để đạt được tốc độ thật của thunderbolt là một hành trình vô cùng gian nan bạn sẽ cần phải mod rom của chipset thunderbolt
{% endhint %}

**Source tham khảo:** [**Hardware Limitations | OpenCore Install Guide (dortania.github.io)**](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#miscellaneous) **|** [**https://vnohackintosh.com/docs/basic-knowledge/limits**](https://vnohackintosh.com/docs/basic-knowledge/limits/) **|** [**https://lzhoang2601.github.io/hardware/hardware-supported**](https://lzhoang2601.github.io/hardware/hardware-supported) **|** [**https://github.com/dortania/bugtracker/issues/192**](https://github.com/dortania/bugtracker/issues/192)
