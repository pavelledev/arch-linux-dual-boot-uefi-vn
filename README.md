# Arch Linux Dual Boot (UEFI)

## Lưu ý quan trọng (hãy đọc hết vì đây không phải hướng dẫn single-boot thông thường):
- Hướng dẫn này dành cho những bạn **gần như mới với Linux**.
- Mình viết guide này với kì vọng các bạn đã biết tạo bootable USB và boot được vào màn hình cài đặt Window và Arch Linux.
- Hướng dẫn chỉ bao gồm các bước cơ bản để cài Windows, phần thiết lập tùy chọn trong Windows sẽ do bạn tự thực hiện.
- Đây không phải hướng dẫn nâng cao nhất, nhưng nó sẽ giúp bạn cài đặt một hệ điều hành Windows (đầy đủ hay tối giản tùy bạn) cùng với Arch Linux tối thiểu.
- Hướng dẫn này **không bao gồm** môi trường Desktop, âm thanh, driver đồ họa, các phần mềm thường sử dụng (những thứ này sẽ đến ở hướng dẫn sau) trên Arch Linux - Bạn sẽ chỉ nhận được một hệ thống gồm Window và Arch Linux cơ bản với một màn hình console màu đen.
- Hướng dẫn này hoạt động cho cả máy bàn và laptop. Người dùng laptop chỉ cần lưu ý mục kết nối mạng Wi-Fi khi cài Arch Linux.



# PHẦN I: WINDOWS
## Lưu ý quan trọng:
Nếu các bạn đã cài Window từ trước thì khả năng cao phân vùng EFI của các bạn đã được tạo mặc định theo tiêu chuẩn mới của Window (100MB), và nó sẽ không đủ cho việc dual boot. Việc thay đổi kích cỡ của phân vùng này gần như là không thể khi Windows đã được cài xong, vì vậy mình khuyên bạn hãy sao lưu các dữ liệu quan trọng và cài lại Windows từ đầu để tạo phân vùng EFI lớn hơn ngay từ bước cài đặt.


## Bước 1: Điều chỉnh kích thước EFI của Windows:
**Mình đoán là các bạn đã biết cài đặt Windows thông thường rồi, và setup này đoán là các bạn có 1 ổ đĩa**
Khi đến bước tùy chỉnh phân vùng của Windows, các bạn hãy xóa hết các phân vùng đang có đi (lưu ý không xóa nhầm USB boot) và làm theo các bước sau:

### 1.1 Mở console:

Bạn nhấn tổ hợp phím [SHIFT] + [F10].

### 1.2 Sử dụng công cụ phân vùng ổ đĩa:

Bạn nhập lệnh sau:

```
diskpart.exe
```

### 1.3 Hiển thị ổ đĩa:

```
list disk
```

### 1.4 Chọn ổ đĩa để phân vùng:

Bạn vui lòng thay `x` bằng ổ đĩa chính.

```
select disk x
```

Ví dụ: `select disk 0`

### 1.5 Tạo phân vùng EFI với kích thước tự chọn:

```
create partition efi size=512
```

Bạn có thể để `1024` để nó là `1GB` cũng được.

### 1.6 Format phân vùng vừa tạo

```
format quick fs=fat32 label=System
```

### 1.7 Thoát khỏi công cụ và console:

```
exit
```
```
exit
```

### 1.8 Làm mới:

Bạn bấm vào nút `Refresh` và bạn sẽ thấy phân vùng EFI mới với kích cỡ đã đặt như trên.
Tại phân vùng ngay bên dưới EFI (phần để cài Win) bạn chọn `Next`


## Bước 2: Cài đặt Window như bình thường

- Phần này mình không hướng dẫn chi tiết vì mỗi người sẽ có tuỳ chọn khác nhau.
- Nếu bạn muốn một hướng dẫn cài Windows tối thiểu, hạn chế tối đa telemetry và data collection, bạn có thể nhắn mình trong cộng đồng Linux nhỏ mà mình lập: [Lều Tuyết](https://discord.gg/mjd6Je4AzA)


## Bước 3: Tách phân vùng cho Arch Linux
**Ví dụ dưới đây dùng ổ ~1TB. Bạn có thể điều chỉnh theo nhu cầu cá nhân!**

### 3.1 Mở công cụ phân vùng

Sau khi đã cài và thiết lập Windows xong, hãy mở công cụ có sẵn trong hệ thống bằng cách nhấn `WIN` rồi tìm::

```
Disk Management
```

Nó trông như này, các bạn cứ coi như phân vùng EFI của nó là `1GB` nhé :v, mình lấy ảnh mạng do đang viết guide và không tiện qua Windows cài lại để chụp ảnh. Nếu các bạn không hiểu cứ nhắn mình bên Discord trên là được.

![Ảnh minh họa Disk Management](/images/diskmansample1.png)

### 3.2 Tách phân vùng cho Arch Linux

Bấm chuột phải vào phân vùng chính mà bạn đã cài Windows và chọn chọn `Shrink Volume`.

![Ảnh minh họa Disk Management](/images/diskmansample2.png)

### 3.3 Nhập số lượng bạn muốn để cho Arch Linux

Tại mục **Enter the amount of space to shrink in MB: ...** nhập số dung lượng bạn muốn tách ra cho Arch Linux. *(Thường thì mình cài hết mọi thứ xong bên Window rồi thì để bù cho nó tầm 20-30GB còn lại cho Arch Linux hết)*

Vì dụ nếu các bạn muốn cho `700GB` thì nhập `700000`. Rồi bấm `Shrink`.



# PHẦN II: ARCH LINUX
Tới đây, bạn đã cài xong Windows và đang boot vào môi trường cài đặt Arch Linux.

## Lưu ý quan trọng:
**Bạn nên đọc kỹ phần này vì quy trình dual-boot khác với single-boot, đặc biệt là ở bước phân vùng và mount (5, 6, 7, 11). Nếu làm sai, bạn có thể khiến Windows không boot được, nên hãy làm cẩn thận từng bước. Tránh việc phải làm lại mọi thứ từ đầu!**

## Mẹo hữu ích:
### Sử dụng tính năng Tab Completion (Tự động hoàn chỉnh lệnh khi TAB)
Hầu hết các lệnh trong Linux đều hỗ trợ tính năng hoàn chỉnh lệnh bằng phím `TAB`:
- Gõ vài kí tự đầu của lệnh hoặc tên tệp.
- Nhấn phím `TAB` để tự động hoàn thành.
- Nếu có nhiều kết quả trùng hợp, nhấn `TAB` hai lần để hiển thị tất cả các gợi ý, hoặc nhấn nhiều lần để chuyển qua từng gợi ý.

Tính năng này giúp giảm lỗi gõ và tăng tốc quá trình cài đặt. Lưu ý rằng không phải tất cả các lệnh đều hỗ trợ Tab Completion.


## Bước 1: Kiểm tra xem hệ thống có đang chạy chuẩn UEFI không

```bash
ls /sys/firmware/efi
```

Nếu hệ thống trả về **No such file or directory**, điều đó có nghĩa là máy tính đăng chạy ở chuẩn **Legacy (BIOS)**, hoặc máy không hỗ trợ **UEFI** (bạn vui lòng vào BIOS kiểm tra).


## Bước 2 (Phụ): Kết nối Wi-Fi
> Nếu bạng đang **dùng kết nối dây LAN (máy bàn/PC)**, bạn có thể bỏ qua bước này.

### 2.1 Kiểm tra Wi-Fi có bị khóa hay không

```bash
rfkill
```

Nếu phần Wi-Fi (thường là `wlan0`) là **hard-blocked**, bạn cần sử dụng công tắc vật lý được cung cấp bởi thiết bị (một vài laptop có công tắc này).
Nếu nó chỉ là **soft-blocked**, bạn có thể gỡ block bằng lệnh sau:

```bash
rfkill unblock wlan
```

### 2.2 Truy cập vào công cụ Wi-Fi (iwd)

```bash
iwctl
```

Hiển thị tất cả các thiết bị dùng để kết nối Wi-Fi:

```text
device list
```

> Chọn thiết bị kết nối Wi-Fi (thường tên là `wlan0`), từ bước này trở đi `wlan0` sẽ được sử dụng trong tất cả các lệnh. Nếu tên thiết bị của bạn khác, vui lòng thay `wlan0` bằng tên thiết bị của bạn.


### 2.3 Quét và kết nối với mạng Wi-Fi

Hiển thị tất cả các Wi-Fi kết nối được băng lệnh sau:

```text
station wlan0 get-networks
```

Kết nối tới mạng Wi-Fi (vui lòng thay `TênWiFi` bằng tên của Wi-Fi bạn đang muốn kết nối):

```text
station wlan0 connect "TênWiFi"
```

Nhập mật khẩu khi được hỏi và bấm **ENTER**.

Thoát công cụ kết nối Wifi:

```text
exit
```


### 2.4 Kiểm tra kết nối mạng

```bash
ping archlinux.org
```

> Nếu có các gói (package) trả về, mạng của bạn đang hoạt động.
> Nhấn tổ hợp phím **CTRL + C** để dừng quá trình ping.


## Bước 3: Đồng bộ thời gian mạng (Network Time Synchronization)

### 3.1 Bật tự đồng bộ thời gian với máy chủ NTP

```bash
timedatectl set-ntp true
```

### 3.2 Kiểm tra trạng thái để đảm bảo việc đồng bộ đang hoạt động

```bash
timedatectl status
```

> Kiểm tra xem dòng `synchronized: yes` thì thời gian đã được đồng bộ.


## Bước 4: Xác định ổ đĩa chính của hệ thống
> Thường được gọi là SSD, HDD,... Trong hướng dẫn này, mình sẽ gọi chung là ổ đĩa hoặc drive.
> Khác với single-boot, lần này khi kiểm tra ổ đĩa bạn sẽ thấy sẵn 4 phân vùng đã được Windows tạo trước: EFI, Windows, và Recovery.

Liệt kê tất cả các ổ đĩa:

```bash
lsblk
```

Ổ đĩa chính của bạn thường có tên như: `nvme0n1`, `sda`, `sdb`,...
- Nếu bạn dùng **SSD NVMe**, nó gần như luôn xuất hiện dưới tên `nvme0n1`.
- Nếu bạn dùng **SSD SATA** hoặc **HDD**, nó thường xuất hiện dưới tên `sda`.


## Bước 5: Phân vùng ổ đĩa (RẤT QUAN TRỌNG VÀ KHÁC VỚI SINGLE-BOOT)
**(Vui lòng đọc toàn bộ bước trước khi thực hiện)**
**Các bạn không được xóa phân vùng 1, 2 và 3 đã được tạo trước bởi Windows!!!**

Mở công cụ chia phân vùng:

```bash
cfdisk /dev/ổ-đĩa-chính
```

Hãy thay `ổ-đĩa-chính` bằng ổ đĩa bạn đã xác định ở **Bước 4**(ví dụ: `nvme0n1` hoặc `sda`,....).
Nếu được hỏi loại bảng phân vùng, hạy chọn **gpt**.

### Cách sử dụng cfdisk:

- Sử dụng các **PHÍM MŨI TÊN** để di chuyển.
- Chọn **Free Space** -> **New** để tạo phân vùng mới.
- Nhập dung lượng mong muốn (ví dụ: `500M`, `30G`,...).
- Chọn phân vùng vừa tạo -> **Type** để đặt loại phân vùng
- Sau khi tạo đầy đủ các phân vùng và đặt đúng loại, chọn **Write**, gõ `yes` và nhấn **ENTER** để lưu.
- Cuối cùng chọn **Quit** để thoát `cfdisk`.

### Gọi ý bố cục phân vùng (Khác với single-boot)
Bạn hãy **xóa phân vùng Recovery phụ** (thường có tên kiểu **Healthy Recovery** hoặc tương tự) — nó thường **nằm bên dưới phân vùng thứ 3**.
Sau khi xoá và bày bố, bố cục lý tưởng sẽ giống bảng này:

| Tên (Name) | Phân vùng (Partition) | Dung lượng (Space) | Loại (Type) |
|------------|-----------------------|--------------------|-------------|
| Phân vùng EFI | nvme0n1p1, sda1,... | 1GB | EFI System |
| Phân vùng Reserve Microsoft | nvme0n1p2, sda2,... | 16M | Microsoft reserved |
| Phân vùng dữ liệu Microsoft | nvme0n1p3, sda3,... | Tùy thiết bị | Microsoft basic data |
| Phân vùng Swap | nvme0n1p4, sda4,... | 4-8G | Linux Swap |
| Phân vùng chính | nvme0n1p5, sda5,... | Phần còn lại | Linux Filesystem |

> Các tên phân vùng sẽ được sử dụng ở **Bước 6** bên dưới!
> **Mẹo:** Nếu bạn không dùng chế độ ngủ đông (Hibernation) của máy, bạn hãy để phân vùng swap khoảng 4-8GB. Nếu bạn muốn sử dụng tính năng ngủ đông, hãy để phân vùng Swap **gấp đôi số RAM của máy bạn** (ví dụ: 16GB RAM -> 32GB Swap).


## Bước 6: Format các phân vùng (RẤT QUAN TRỌNG VÀ KHÁC VỚI SINGLE-BOOT)

### 6.1 Format phân vùng Swap

```bash
mkswap /dev/phân-vùng-swap
```
```bash
swapon /dev/phân-vùng-swap
```

> Thay `/dev/phân-vùng-swap` với phân vùng Swap của bạn tại **Bước 4** (ví dụ: `/dev/nvme0n1p4`, `/dev/sda4`,... )

### 6.2 Format phân vùng chính

```bash
mkfs.ext4 /dev/phân-vùng-chính
```

> Thay `/dev/phân-vùng-chính` với phân vùng chính của bạn tại **Bước 4** (ví dụ: `/dev/nvme0n1p5`, `/dev/sda5`,... )
> **Lưu ý quan trọng:** Các bạn vui lòng kiểm tra kĩ tên phân vùng để tránh lỗi và mất dữ liệu.


## Bước 7: Gán các phân vùng (Mount) (RẤT QUAN TRỌNG VÀ KHÁC VỚI SINGLE-BOOT)
> Từ bước này trở đi mình khá chắc các bạn đã hiểu rõ các phân vùng là gì.
> Để cho rõ ràng, mình sẽ không nhắc lại cấu trục/tên của mỗi phân vùng mỗi lệnh nữa.

### 7.1 Gán phân vùng chính
> Trong trường hợp dual boot thì nó hay là phân vùng thứ 5.

```bash
mount /dev/phân-vùng-chính /mnt
```

### 7.2 Tạo các điểm gán cho EFI, thư mục /home và điểm gán cho Windows

```bash
mkdir -p /mnt/boot
```
```bash
mkdir -p /mnt/home
```
```bash
mkdir -p /mnt/windows
```

### 7.3 Gán phân vùng EFI
> Trong trường hợp dual boot thì nó hay là phân vùng thứ 1.

```bash
mount /dev/phân-vùng-efi /mnt/boot
```

### 7.4 Gán phân vùng cho Windows
> Trong trường hợp dual boot thì nó hay là phân vùng thứ 3.

```bash
mount /dev/phân-vùng-dữ-liệu-microsoft /mnt/windows
```


## Bước 8: Cài đặt các gói cơ bản
> Hãy đọc toàn bộ bước này trước khi thực hiện!

Cài đặt các gói cần thiết cho hệ thống gốc của Arch Linux:

```bash
pacstrap /mnt base base-devel linux linux-firmware linux-headers vim
```

- Nếu bạn dùng **CPU Intel**, hãy thêm gói này vào sau cùng của lệnh trên:
```bash
intel-ucode
```

- Nếu bạn dùng **CPU AMD**, hãy thêm gói này vào sau cùng của lệnh trên:
```bash
amd-ucode
```


## Bước 9: Cài đặt hệ thống cơ bản
**Bước này thiết lập cấu hình hệ thống cơ bản cho Arch Linux, hãy làm theo thật kỹ!**

### 9.1 Tạo Fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### 9.2 Truy cập vào hệ thống mới

```bash
arch-chroot /mnt
```

### 9.3 Đặt múi giờ (Timezone)

```bash
ln -sf /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime
```

### 9.4 Đồng bộ thời gian phần cứng

```bash
hwclock --systohc
```

### 9.5 Cấu hình locale
> Mẹo sử dụng **vim** sẽ được giải thích ở cuối bước 9.5

1. Mở file cấu hình locale:

```bash
vim /etc/locale.gen
```

2. Bỏ dấu `#` ở dòng sau (uncomment):
> Lưu ý là `en_US` chứ không phải `es_US` nhé, có người từng nhầm rồi!
> Mình giải thích `uncomment` ở cuối bước 9.5

```text
en_US.UTF-8
```

Lưu vào thoát (giải thích ở cuối bước 9.5)

3. Tạo locale:

```bash
locale-gen
```

4. Đặt locale mặc định cho hệ thống

```bash
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

> **Mẹo sử dụng vim dành cho người mới:**
> - Nhấn phím `[I]` (chữ i, không phải L) để vào chễ độ nhập chữ.
> - Xóa dấu `#` ở đầu dòng để uncomment.
> - Nhấn phím `[ESC]`, gõ `:wq` và `[ENTER]` để lưu và thoát hoặc `:q!` để thoát không lưu.

### 9.6 Đặt tên thiết bị

1. Đặt tên thiết bị:

```bash
echo tên-thiết-bị > /etc/hostname
```

> Thay `tên-thiết-bị` bằng tên bạn muốn, ví dụ: `pavelle-pc`.

2. Chỉnh sửa file `/etc/hosts`:

```bash
vim /etc/hosts
```

- Thêm dòng sau (dùng phím `[TAB]` tại vị trí `<TAB>` xuất hiện), và nhớ thay `tên-thiết-bị` bằng tên thiết bị bạn đã đặt ở bước trên:

```text
127.0.1.1<TAB>tên-thiết-bị.localdomain<TAB>tên-thiết-bị
```

Lưu vào thoát

### 9.7 Đặt mật khẩu hệ thống

```bash
passwd
```

> Nhập mật khẩu hệ thống và lặp lại *(nó sẽ tàng hình trong lúc nhập)*.


## Bước 10: Tạo tài khoản người dùng

### 10.1 Thêm người dùng mới

```bash
useradd -m tên-người-dùng
```

> Thay `tên-người-dùng` bằng tên bạn muốn.
> Từ bước này trở đi, thay `tên-người-dùng` bằng tên mà bạn đã đặt (10.1)

### 10.2 Đặt mật khẩu người dùng

```bash
passwd tên-người-dùng
```

> Nhập mật khẩu người dùng và lặp lại *(nó sẽ tàng hình trong lúc nhập)*.

### 10.3 Thêm người dùng vào các nhóm phổ biến

```bash
usermod -aG wheel,audio,video,optical,storage,power tên-người-dùng
```

### 10.4 Bật quyền sudo cho người dùng
> Cách sử dụng vim đã được giải thích ở cuối Bước 9.

```bash
EDITOR=vim visudo
```

- Thêm dòng sau ngay dưới dòng `root ALL=(ALL) ALL`:

```text
tên-người-dùng ALL=(ALL) ALL
```

- Uncomment dòng sau *(nó ở ngay dưới chỗ bạn đang ở luôn)*:

```text
%wheel ALL=(ALL:ALL) ALL
```

Lưu vào thoát


## Bước 11: Cài đặt bootloader (GRUB) (RẤT QUAN TRỌNG VÀ KHÁC VỚI SINGLE-BOOT)

### 11.1 Cài GRUB và công cụ mạng

```bash
pacman -S grub efibootmgr osprober networkmanger network-manager-applet
```

### 11.2 Bật NetworkManager

```bash
systemctl enable NetworkManager
```

### 11.3 Cài GRUB vào phân vùng EFI
> Hãy cẩn thận với bước này vì rất dễ gõ sai lệnh!

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

### 11.4 Cho phép GRUB sử dụng OS-PROBER
> Cách sử dụng vim đã được giải thích ở cuối Bước 9.

```
vim /etc/default/grub
```

Các bạn xuống dưới cùng và `uncomment` dòng sau:

```text
GRUB_DISABLE_OS_PROBER=false
```

Lưu vào thoát

### 11.5 Tạo cài đặt GRUB

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

Nếu bạn thấy dòng sau thì bạn đã thành công thêm **Windows** vào GRUB:

```text
Found Windows Boot Manager on /dev/.../EFI/Microsoft/Boot/bootmgfw.efi
```

Nếu không thấy, sau khi làm xong bước 12 bạn hãy chạy lại lệnh này:

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```


## Bước 12: Thoát, khởi động lại và đăng nhập vào hệ thống mới

- Thoát khỏi hệ thống vừa được cấu hình xong:

```bash
exit
```

- Bỏ gán các phân vùng:

```bash
umount -R /mnt
```

- Khởi động lại máy

```
reboot
```

- Lần này tại **GRUB** bạn sẽ thấy thêm lựa chọn để vào **Windows** tại dòng 3 *(thường thì là dòng 3)*.
- Sau khi khởi động xong, bạn sẽ được yêu cầu nhập `tên-người-dùng` và `mật-khẩu` bạn đã đặt tại **Bước 10** để đăng nhập vào hệ thống mới.


## Bước 13: Đồng bộ thời gian với Windows

```
sudo timedatectl set-local-rtc 1
```


## Bước 14 (Phụ): Kết nối Wi-Fi lại

- Mở công cụ quản lý mạng:

```bash
sudo nmtui
```

- Nhập mật khẩu của bạn
- Từ đây trở đi, công cụ này đã khá dễ sử dụng rồi, bạn chỉ cần dùng các **PHÍM MŨI TÊN** để di chuyển như khi dùng `cfdisk` thôi.


## Bước 15: Cập nhật hệ thống
> Bạn nên thực hiện bước này hàng ngày vì Arch Linux là một update thường xuyên. Nếu lâu không cập nhật, có thể gặp sự cố!

```
sudo pacman -Syu
```


# Chúc mừng! 🎉

Bạn đã thành công cài đặt một hệ thống **Arch Linux tối thiểu nhất** và thiết lập **dual boot với Windows!**
Đây chỉ là hệ thống cơ bản. Mình sẽ tạo thêm các hướng dẫn về cách xây dựng một hệ thống **Arch Linux hoàn chỉnh**, đầy đủ chức năng, và sẽ để link tới các hướng dẫn đó khi có sẵn.
Tài liệu và công cụ mình để tại Discord: [Lều Tuyết](https://discord.gg/mjd6Je4AzA)