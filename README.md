# Nghĩa AI OCR WebApp

<img src="logo.png" alt="Nghĩa AI" width="100">

Ứng dụng web trích xuất văn bản từ ảnh hóa đơn, phiếu giao hàng, đơn hàng và các tài liệu chụp bằng điện thoại. Ứng dụng sử dụng **Google Cloud Vision** để nhận dạng chữ, hiển thị kết quả trên trình duyệt và xuất tệp văn bản để tiếp tục xử lý.

**Phát triển bởi:** Võ Trọng Nghĩa — Nghĩa AI  
**Website:** [nghiaai.com](https://nghiaai.com)  
**Quyền sử dụng mã nguồn:** [Thông báo bản quyền và bảo lưu quyền](LICENSE)

## Tính năng hiện có

- Tải lên **một ảnh JPG, JPEG hoặc PNG** và xem trước ảnh.
- Nhận dạng văn bản bằng Google Cloud Vision `DOCUMENT_TEXT_DETECTION`, với gợi ý ngôn ngữ tiếng Việt và tiếng Anh.
- Hiển thị văn bản OCR để người dùng đọc và sao chép.
- Tải kết quả về dưới dạng **`ocr.txt`**.
- Nhập Google Vision API key trên giao diện; ô nhập được che ký tự.
- Có các trường nhập tên nhân viên và chọn loại tài liệu: Hóa đơn, Đơn hàng, Phiếu giao, Khác.

Giới hạn mỗi ảnh trong mã ứng dụng là **4 MiB**, tương đương `4 × 1024 × 1024` byte. Đây là giới hạn do ứng dụng đặt ra.

Các trường tên nhân viên và loại tài liệu hiện chỉ có trên giao diện; chưa được lưu hoặc đưa vào tệp kết quả.

## Công nghệ và các tệp chính

| Thành phần | Vai trò |
| --- | --- |
| Python | Ngôn ngữ lập trình |
| Streamlit | Giao diện web và bộ nhớ đệm kết quả |
| Requests | Gửi yêu cầu HTTP tới Google Cloud Vision |
| Google Cloud Vision | Dịch vụ nhận dạng văn bản từ ảnh |
| [`app.py`](app.py) | Giao diện, xử lý ảnh và gọi OCR |
| [`requirements.txt`](requirements.txt) | Danh sách thư viện cần cài |
| [`logo.png`](logo.png) | Logo hiển thị trong ứng dụng |
| [`LICENSE`](LICENSE) | Phạm vi quyền đối với mã nguồn |

## Cài đặt trên máy

Hướng dẫn này dành cho chủ sở hữu hoặc người được cấp quyền sử dụng mã nguồn theo [LICENSE](LICENSE).

### 1. Chuẩn bị

- Python tương thích với Streamlit; có thể chọn **Python 3.11 hoặc 3.12** cho môi trường mới. Xem [hướng dẫn cài đặt Streamlit](https://docs.streamlit.io/get-started/installation/command-line).
- Kết nối Internet để cài thư viện và gọi dịch vụ OCR.
- Google Cloud project đã bật **Cloud Vision API**, cấu hình thanh toán và API key được phép gọi dịch vụ.

Xem [thiết lập Cloud Vision](https://docs.cloud.google.com/vision/docs/setup) và [hướng dẫn quản lý API key](https://docs.cloud.google.com/docs/authentication/api-keys-best-practices).

Repo hiện chưa khóa phiên bản thư viện trong `requirements.txt` và chưa công bố ma trận phiên bản đã kiểm thử.

### 2. Lấy mã nguồn

```bash
git clone https://github.com/nghiaai-lab/ocr-webapp.git
cd ocr-webapp
```

Có thể tải ZIP và giải nén nếu không dùng Git. Các lệnh tiếp theo phải chạy trong thư mục chứa `app.py`, `logo.png` và `requirements.txt`.

### 3. Tạo môi trường và cài thư viện

**Windows — PowerShell:**

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

**macOS / Linux:**

```bash
python3 -m venv .venv
./.venv/bin/python -m pip install -r requirements.txt
```

Các lệnh sử dụng trực tiếp Python trong môi trường riêng, không cần kích hoạt môi trường bằng script.

### 4. Chuẩn bị cấu hình API key

Trong bản sao trên máy, tạo thư mục `.streamlit` nếu chưa có. Tạo hoặc chỉnh tệp `.streamlit/secrets.toml` để trường cấu hình này có giá trị rỗng:

```toml
GCV_API_KEY = ""
```

Với cấu hình này, người chạy nhập API key của mình trên giao diện khi sử dụng.

Mã hiện tại đọc `st.secrets` trước khi tạo ô nhập API key. Vì vậy, vẫn cần tệp cấu hình hợp lệ ngay cả khi muốn nhập khóa bằng tay. Nếu tạo tệp trong lúc ứng dụng đang chạy, hãy khởi động lại Streamlit.

**Không đưa khóa thật lên GitHub.** Nếu tự tạo `.gitignore`, nên có các mục:

```gitignore
.venv/
__pycache__/
.streamlit/secrets.toml
```

`.gitignore` không tự loại bỏ tệp đã được Git theo dõi. Nếu khóa từng bị đưa lên repo, cần thu hồi hoặc thay khóa và xử lý tệp/lịch sử theo [hướng dẫn GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository).

### 5. Chạy ứng dụng

**Windows — PowerShell:**

```powershell
.\.venv\Scripts\python.exe -m streamlit run app.py
```

**macOS / Linux:**

```bash
./.venv/bin/python -m streamlit run app.py
```

Mở địa chỉ **Local URL** được Streamlit in ra trong terminal. Nhấn **Ctrl+C** trong terminal để dừng ứng dụng.

## Cách sử dụng

1. Mở ứng dụng và nhập thông tin người gửi nếu cần.
2. Nhập Google Vision API key của mình.
3. Chọn một ảnh JPG/JPEG/PNG không vượt giới hạn dung lượng.
4. Ứng dụng **tự bắt đầu OCR khi đã có ảnh hợp lệ và API key**; giao diện hiện không có nút bắt đầu riêng.
5. Đọc kết quả, đối chiếu với ảnh gốc và chọn **Tải TXT** để lưu `ocr.txt`.

Nên sử dụng ảnh rõ chữ, đủ sáng và ít nghiêng. Kiểm tra lại tên, ngày tháng, số lượng và số tiền trước khi đưa kết quả vào nghiệp vụ.

Tệp TXT chứa văn bản OCR gốc. Việc chỉnh sửa trực tiếp trong ô kết quả hiện chưa cập nhật nội dung tệp tải xuống; hãy chỉnh sửa tệp sau khi tải về nếu cần.

## Dữ liệu, chi phí và phạm vi sử dụng

- **Ảnh được gửi tới Google Cloud Vision** để xử lý. Luồng OCR cần Internet và không chạy hoàn toàn trên máy.
- Mã nguồn cấu hình bộ nhớ đệm OCR với `ttl=3600`, tương đương một giờ, theo nội dung ảnh và API key. Khi đầu vào trùng, ứng dụng có thể dùng lại kết quả. Cache này chưa được tách theo tài khoản người dùng.
- Ứng dụng chưa có cơ sở dữ liệu hoặc chức năng lưu lịch sử OCR.
- Chi phí dịch vụ OCR được tính theo tài khoản Google Cloud và mức sử dụng. Xem [bảng giá Cloud Vision](https://cloud.google.com/vision/pricing) trước khi chạy số lượng lớn.
- Khi chạy bản web do người khác vận hành, ảnh và API key nhập trên giao diện được xử lý tại máy chủ đó. Chỉ sử dụng máy chủ mà mình tin cậy.

**Trước khi triển khai cho nhiều người:** cần bổ sung kiểm soát truy cập, quản lý khóa ở phía máy chủ và hạn mức sử dụng. Mã hiện tại đưa giá trị `GCV_API_KEY` từ cấu hình vào ô nhập trên trình duyệt; che ký tự không phải là cơ chế giữ kín khóa của máy chủ. Không cấu hình khóa dùng chung làm giá trị mặc định cho một ứng dụng công khai.

Tham khảo [quản lý secrets của Streamlit](https://docs.streamlit.io/develop/concepts/connections/secrets-management) và [cơ chế cache](https://docs.streamlit.io/develop/api-reference/caching-and-state/st.cache_data).

## Những chức năng chưa có trong phiên bản hiện tại

- Nhận PDF, xử lý nhiều ảnh cùng lúc hoặc tự theo dõi thư mục.
- Bóc tách trường hóa đơn, bảng hàng hóa hay dữ liệu có cấu trúc.
- Xuất Excel/CSV, ghi Google Sheets hoặc kết nối Make/webhook.
- Đăng nhập, phân quyền người dùng, quản lý hồ sơ và thanh toán.

Repo hiện chưa có bộ kiểm thử tự động hoặc kết quả đo độ chính xác OCR. Nội dung README mô tả mã nguồn hiện có, không phải xác nhận ứng dụng đã được kiểm thử để vận hành thương mại.

## Xử lý một số lỗi thường gặp

| Hiện tượng | Cách kiểm tra |
| --- | --- |
| Không tìm thấy thư viện `streamlit` hoặc `requests` | Cài lại bằng lệnh Python trong `.venv` như phần hướng dẫn; chạy ứng dụng bằng cùng môi trường. |
| Báo thiếu secrets hoặc lỗi đọc cấu hình | Kiểm tra `.streamlit/secrets.toml`, cú pháp `GCV_API_KEY = ""` và thư mục đang chạy; khởi động lại Streamlit. |
| Không tìm thấy `logo.png` | Chạy lệnh trong thư mục chứa `app.py` và giữ `logo.png` ở cùng thư mục. |
| Báo ảnh vượt 4 MB | Giảm dung lượng ảnh xuống không quá 4 MiB rồi tải lại. |
| API trả lỗi hoặc yêu cầu bị timeout | Kiểm tra Internet, API key, trạng thái bật Vision API, hạn chế của khóa, thanh toán và hạn mức Google Cloud. Yêu cầu HTTP hiện được cấu hình `timeout=30`. |
| Không tìm thấy văn bản | Thử ảnh rõ chữ hơn. Mã hiện chưa xử lý riêng mọi lỗi nằm trong nội dung phản hồi API, nên thông báo này chưa đủ để kết luận ảnh không có chữ. |

## Bản quyền và liên hệ

Phần mã nguồn và tài liệu thuộc quyền tác giả của **Võ Trọng Nghĩa** được bảo lưu quyền theo [LICENSE](LICENSE). Việc repo có thể được xem công khai không tự cấp giấy phép nguồn mở cho phần nội dung này.

Các thành phần của bên thứ ba tiếp tục tuân theo giấy phép riêng tương ứng. Quyền sử dụng ứng dụng của khách hàng được xác định theo điều khoản hoặc thỏa thuận riêng.

Liên hệ về quyền sử dụng mã nguồn và hợp tác thương mại: **[Nghĩa AI — nghiaai.com](https://nghiaai.com)**.

