# Aethmere · 识海

> Kho phân phối công khai — **đây không phải là kho mã nguồn mở**.

[English](../../README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | **Tiếng Việt** | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | [Português](README.pt.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere là một lớp bộ nhớ dành cho công việc có AI hỗ trợ, xem **không bịa đặt**
là một yêu cầu kỹ thuật chứ không phải một khẩu hiệu. Nó mang lại cho các ứng dụng
AI được hỗ trợ một bộ nhớ bền vững, do người dùng kiểm soát, với ranh giới câu trả
lời rõ ràng: điều bạn đã yêu cầu ghi nhớ một cách tường minh sẽ được trả lời chính
xác; điều chưa từng được ghi lại, hoặc đã bị thu hồi, sẽ bị từ chối thay vì phỏng
đoán; các câu hỏi thông thường được chuyển thẳng tới mô hình của bạn, nguyên vẹn.

[Website](https://aethmere.com) ·
[Ứng dụng web](https://app.aethmere.com) ·
[Bản phát hành mới nhất](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Báo lỗi](https://github.com/kzkz137806/aethmere-os/issues)

## Vì sao chọn Aethmere

Phần lớn hệ thống bộ nhớ AI thất bại theo một trong hai hướng: chúng bịa ra những
"ký ức" bạn chưa từng cung cấp, hoặc nuốt mất các câu hỏi thông thường bằng những
lần từ chối không cần thiết. Làn bộ nhớ được quản trị của Aethmere được xây dựng
sao cho không hướng nào có thể ẩn mình:

- **Câu hỏi trả lời được thì phải được trả lời chính xác.** Từ chối một câu hỏi trả lời
  được bị tính là thất bại trong đánh giá của chúng tôi — độ chính xác không bao giờ
  có thể mua được bằng cách từ chối.
- **Câu hỏi không trả lời được thì phải bị từ chối.** Nếu một giá trị chưa từng được
  ghi lại, đã bị thu hồi, hoặc mơ hồ, thì đưa ra *bất kỳ* giá trị nào cũng là bịa đặt.
  Làn được quản trị sẽ từ chối một cách tất định.
- **Câu hỏi thông thường thì phải được cho đi qua.** Một câu hỏi chỉ đơn thuần nhắc đến
  các từ ngữ về bộ nhớ sẽ được định tuyến tới mô hình của bạn, chứ không bị nuốt mất.
- **Ghi nhớ phải được xác nhận.** Một tin nhắn trông giống lệnh ghi nhớ chỉ được ghi
  sau khi bạn xác nhận tường minh; nếu bạn từ chối, tin nhắn đó vẫn chỉ là lịch sử
  trò chuyện thông thường.

## Kết quả đo được (đánh giá niêm phong, có giới hạn phạm vi)

Trong một đánh giá nội bộ được niêm phong đối với hợp đồng bộ nhớ được quản trị —
hệ thống ứng viên được đóng băng bằng hàm băm trước khi rút ra hạt giống ngẫu nhiên
đã cam kết, các ca kiểm thử được sinh ra một cách tất định, mọi câu trả lời được chấm
bởi một bộ phán định máy cố định ngay tại thời điểm sinh đề, toàn bộ biên nhận được
lưu giữ:

| Chỉ số | Kết quả | Cận dưới 95% |
|---|---|---|
| Độ chính xác có giới hạn | **2,400 / 2,400 cụm đúng** (8 họ nhiệm vụ × 300, không dung sai cho mỗi họ) | ≥ 99.87% |
| Chữa ảo giác có giới hạn | **1,800 / 1,800 lỗi của đường cơ sở được sửa, 0 / 600 hồi quy** so với một mô hình 7B chạy cục bộ nhận cùng các cuộc hội thoại nhưng không có quản trị | ≥ 99.83% |

Tám họ nhiệm vụ bao phủ: hồi tưởng trực tiếp, tập hợp và đếm, hồi tưởng theo phạm vi
thời gian, cập nhật và xung đột, nối nhiều bước, áp lực ký ức giả (nơi mọi giá trị
được đưa ra đều sẽ là bịa đặt), ghi chú khóa–giá trị mở, và áp lực ranh giới (những
câu trần thuật không được phép nạp vào, và những câu hỏi thông thường không được phép
bị nuốt). Trên cùng những cuộc hội thoại đó, đường cơ sở 7B cục bộ không có quản trị
đã bịa đặt hoặc trả lời sai ở 75% số cụm; làn được quản trị đã sửa toàn bộ, không có hồi quy
nào trên những cụm mà đường cơ sở trả lời đúng.

**Phạm vi, nói thẳng:** đây là những kết quả có giới hạn trên hợp đồng bộ nhớ được
quản trị của Aethmere — tức ngữ pháp lệnh tường minh và các họ truy vấn của nó — được
đo đầu-cuối xuyên qua các dịch vụ thực tế nạp vào và đưa ra giá trị bộ nhớ. Chúng không phải là
một tuyên bố về thế giới mở, không phải một tuyên bố về độ chính xác của toàn bộ sản
phẩm, và không phải một tuyên bố về các câu trả lời tổng quát của mô hình bạn dùng.
Bên ngoài hợp đồng được quản trị, mô hình của bạn trả lời như thường lệ và các giới
hạn thông thường của mô hình vẫn hiện diện.

## Aethmere làm được gì

**Bộ nhớ được quản trị (phần lõi)**

- Các lệnh ghi nhớ tường minh với ngữ nghĩa chính xác, có thể kiểm toán: ghi lại,
  cập nhật, thu hồi, định vị, và ghi chú khóa–giá trị mở; tập hợp nhiều giá trị;
  hồi tưởng theo phạm vi thời gian.
- Chuỗi nguồn gốc bộ nhớ có ký số: mỗi dữ kiện được chấp nhận đều mang một chuỗi có
  thể kiểm chứng truy ngược về tin nhắn gốc; giá trị đã thu hồi không bao giờ xuất
  hiện lại qua bất kỳ truy vấn nào.
- Xác nhận trước khi ghi: các lệnh ghi nhớ mới cần bạn xác nhận tường minh trong sản
  phẩm trước khi bất cứ điều gì được lưu lại.
- Thu nhận câu tự do kèm kiểm chứng cục bộ: các câu tự nhiên có thể đề cử ứng viên bộ
  nhớ thông qua một mô hình cục bộ và được kiểm chứng lại một cách tất định trước khi
  được chấp nhận — và không có bất kỳ dữ liệu gốc nào của bạn rời khỏi máy.

**Bộ nhớ đám mây cá nhân**

- Không gian đám mây cô lập theo tài khoản (khoảng 100M token ước tính, trải trên tối
  đa 200 cuộc hội thoại) với khôi phục xuyên thiết bị; công tắc tải lên riêng cho từng thiết bị; câu
  trả lời chỉ chèn phần lịch sử liên quan và có giới hạn — không bao giờ nạp toàn bộ
  kho lưu trữ.
- Khóa API của nhà cung cấp được lưu dưới dạng bản mã AES-GCM gắn với tài khoản của
  bạn; các API thông thường chỉ nhìn thấy bốn ký tự cuối.

**Tài liệu và hình ảnh**

- Cơ sở tri thức tài liệu: TXT, Markdown, CSV, JSON, HTML và PDF; văn bản được trích
  xuất ngay trong trình duyệt của bạn và chỉ các mảnh truy hồi cô lập theo tài khoản
  cùng một chỉ mục vector lai được lưu lại — tệp gốc không được giữ.
- OCR hình ảnh: văn bản trích xuất được chèn kèm tiền tố nguồn và một bản tóm tắt
  "cần rà soát"; việc nhận dạng chạy qua nhà cung cấp mà bạn đã cấu hình.

**Tìm kiếm thời gian thực**

- Tìm kiếm web thời gian thực đa công cụ với cửa sổ thời gian (ngày / vài ngày / tuần
  / tháng), tự động lập kế hoạch truy vấn và thử lại, cùng giới hạn số kết quả được
  tinh chỉnh để câu trả lời có căn cứ.
- Truy hồi xuyên ngôn ngữ: các câu hỏi tiếng Trung được tự động ánh xạ sang những chủ
  đề tìm kiếm quốc tế tập trung (thị trường, hàng hóa, tiền tệ và hơn thế nữa).
- Ảnh chụp nhanh thị trường hợp đồng tương lai Trung Quốc theo thời gian thực cho các
  mã được hỗ trợ, lấy về ngay lúc trả lời và được trích dẫn như nguồn dữ liệu trong
  câu trả lời.

**Ở mọi nơi bạn làm việc**

- Ứng dụng web di động/máy tính có thể cài đặt (PWA) với câu trả lời dạng phát trực
  tiếp, khối mã, bảng biểu và sao chép tin nhắn chỉ bằng một lần chạm.
- CLI trên máy tính (`aethmere-cli`) với liên kết thiết bị dùng một lần: `aethmere sync`
  phản chiếu bộ nhớ đám mây của bạn về máy; Claude Code, Codex và các ứng dụng MCP khác
  có thể dùng nó qua `cloud_memory_recall`. Mặc định chỉ đọc; tải lên đòi hỏi bật
  đồng ý hai lần một cách tường minh.
- Kênh trò chuyện: liên kết Telegram (nhắn riêng với bot) hoặc Discord (`/aethmere ask`,
  trả lời chỉ người hỏi thấy) với tài khoản của bạn bằng mã dùng một lần; hủy liên kết
  cắt quyền truy cập ngay lập tức.
- Kho kỹ năng phía máy chủ: các thẻ năng lực được tuyển chọn sẽ tự động được định tuyến
  sau khi đăng nhập — không cần đấu nối kỹ năng thủ công.

## Cài đặt Aethmere CLI

Yêu cầu: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Phiên bản mong đợi:

```text
Aethmere CLI 0.7.0
```

`aethmere connect` tạo một kết nối ở cấp người dùng cho các ứng dụng AI được hỗ trợ.
Bạn không cần kết nối lại mỗi khi đổi thư mục dự án. Dùng cục bộ không cần lời mời
trên web. Đăng nhập và đồng bộ đám mây là tùy chọn, và việc tải lên từ máy tính vẫn
tắt cho đến khi người dùng bật.

Để xem hướng dẫn từng bước bằng tiếng Trung, hãy truy cập
[aethmere.com](https://aethmere.com/#install).

## Kiểm chứng bản tải về

SHA-256 của `aethmere-cli-0.7.0.tgz`:

```text
964903d1f5787e6fb58dfe37a762d29c966971abd20e06a2b22cdcfe9954a2a6
```

PowerShell:

```powershell
Get-FileHash .\aethmere-cli-0.7.0.tgz -Algorithm SHA256
```

macOS/Linux:

```bash
shasum -a 256 aethmere-cli-0.7.0.tgz
```

CLI cũng kiểm chứng siêu dữ liệu cập nhật đã ký, kích thước gói và SHA-256 trước khi
một bản cập nhật được cài đặt. Cập nhật không bao giờ được cài nếu chưa xác nhận.

## Kho này chứa những gì

Kho công khai này là nơi chính thức cho:

- các bản tải về và checksum;
- hướng dẫn cài đặt và cập nhật;
- nhật ký thay đổi công khai;
- theo dõi sự cố và báo cáo bảo mật.

Phần lõi độc quyền của Aethmere, các hệ thống tri thức riêng tư, tài liệu đánh giá,
phần hiện thực dịch vụ và lịch sử phát triển nội bộ **không được bao gồm**.

## Mô hình sản phẩm

Aethmere sử dụng mô hình ứng dụng công khai/lõi riêng tư:

- các điểm vào phân phối và tích hợp công khai;
- các dịch vụ lõi độc quyền được vận hành trên máy chủ của chúng tôi;
- ứng dụng người dùng có thể tải về;
- không công bố công khai mã nguồn của phần lõi.

Nội dung của kho này và các sản phẩm phát hành kèm theo là tài sản độc quyền, trừ khi
một tệp nêu rõ điều khác. Không có giấy phép mã nguồn mở nào được cấp. Xem
[NOTICE.md](NOTICE.md).

## Hỗ trợ

Hãy dùng [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) cho các báo
cáo lỗi và đề xuất tính năng công khai. Đừng đưa vào mật khẩu, khóa API, ký ức riêng
tư, dữ liệu cá nhân hay nội dung dự án bảo mật.

Với các vấn đề bảo mật, hãy làm theo [SECURITY.md](SECURITY.md).
