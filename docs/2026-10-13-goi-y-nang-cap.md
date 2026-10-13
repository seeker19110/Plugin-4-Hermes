# Gợi ý nâng cấp — rà soát 2026-10-13

> Lấy từ đợt rà soát chéo 19 repo của chủ sở hữu (ma trận vệ sinh ở [seeker19110/stored, mục 23 §8.1](https://github.com/seeker19110/stored/blob/main/23-own-repos-lessons/README.md); gợi ý tích hợp ở §7b của cùng file). Số liệu đo bằng lệnh trên HEAD (đếm `uses:` ghim SHA 40 ký tự, khối `permissions:`, `dependabot.yml`, `.claude/settings.json`), không suy ra từ văn xuôi. Đây là **đề xuất**, chưa sửa code. Mỗi mục kèm bằng chứng để kiểm lại trước khi làm; mục nào đã xử lý thì đánh dấu hoặc xoá.

## Hiện trạng đo được

| HEAD đọc | Action ghim SHA | Workflow thiếu `permissions:` cấp đầu | Dependabot | LICENSE · SECURITY | File test |
|---|---|---|---|---|---|
| `2d922b0` (2026-08-31) | không có workflow | – | **không có** | có · không | 2 |

## Đề xuất (🔴 cao · 🟠 trung bình · 🟢 nhẹ)

| # | Mức | Đề xuất | Bằng chứng | Kiểm xong khi |
|---|---|---|---|---|
| 1 | 🔴 | Port lớp `bridge/http_security.py` từ `donghanhcungban/hermes-agents` (kiểm loopback và Host/Origin, giới hạn body 2 MB, tối đa 8 request đồng thời, token admin so bằng `hmac.compare_digest`), **hoặc archive repo** và trỏ README sang Hermes-Agents | `bridge/server.py` (290 dòng) là bản tiền thân, không kiểm Host / Origin, và không bắt buộc token, nên dù bind `127.0.0.1` một trang web độc hại vẫn có thể gọi tới qua DNS-rebinding; Hermes-Agents đã vá bằng `http_security.py` (139 dòng) | Request có `Host: evil.example` bị trả 403 |
| 2 | 🟢 | Truyền `host` / `port` qua `sys.argv` thay vì nội suy vào chuỗi mã Python ở `manage.py:88` | `manage.py:88` dựng `run_server(host='{host}', …)` bằng f-string; Hermes-Agents đã chuyển sang `sys.argv[1]` | Không còn f-string chứa `{host}` trong chuỗi mã |
| 3 | 🟢 | Thêm CI tối thiểu chạy `tests/test_failover.py` (action ghim SHA) và thêm SECURITY.md | Repo không có workflow nào | Có CI xanh |

## Giữ nguyên (điểm mạnh)

- Cấu trúc plugin Hermes rõ ràng (`plugin/plugin.yaml`) và README song ngữ.

## Gợi ý tích hợp từ catalog `stored`

- codegraph liệt kê Hermes Agent là host được hỗ trợ (stored mục 16 §2d).

## Ghi chú chung

- Ghim action theo SHA bằng [pinact](https://github.com/suzuki-shunsuke/pinact) hoặc [ratchet](https://github.com/sethvargo/ratchet), giữ comment tag (`# v7.0.1`), rồi để Dependabot `github-actions` tự nâng SHA. Đây là cách projects-template, xboss và Sales-Hunter đang làm.
- Chuẩn tham chiếu là `seeker19110/projects-template`: 25/25 action ghim SHA, mọi workflow có `permissions:` cấp đầu, Dependabot cho npm, github-actions và pip.
