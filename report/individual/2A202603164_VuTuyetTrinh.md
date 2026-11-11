# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202603164
- Ngày / CVAT local: 11–12/11/2026 / CVAT local v2.74.1 (Docker Desktop + WSL)
- Công cụ đã dùng: CVAT Polygon và Brush. Không dùng SAM: đã thử deploy SAM ViT-B qua Nuclio nhưng build bị kẹt ở trạng thái `building`, nên bỏ và vẽ tay.

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `submissions/easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `submissions/medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `submissions/hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `submissions/cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `submissions/cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `submissions/cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `submissions/cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `submissions/cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `submissions/cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Cả 9 ZIP đạt `OK` khi chạy `scripts/inspect_submissions.py` (chỉ kiểm cấu trúc, không kiểm độ đúng của mask). `medium_instance`: 70 object trên 3 ảnh: 31 (`000000181542.jpg`), 22 (`000000373353.jpg`), 17 (`000000458325.jpg`). Bản export đầu thiếu ảnh `000000458325.jpg`; đã vẽ bổ sung, Save và export lại.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg`, người đứng ở khoảng giữa-trái ảnh (bbox x≈188–305, y≈131–461 trên ảnh 640×600), vẽ bằng Polygon.
- Class và quy tắc tôi dùng để chọn biên: `person`. ✏️ *[Bạn điền: vì sao dừng biên ở đó, ví dụ chỉ vẽ phần nhìn thấy, không vẽ phần bị che, có tính túi xách/đồ cầm tay không.]*
- Nếu dùng gợi ý sau đó: không dùng (SAM không deploy được).
- Quyết định gán nhãn của tôi: ✏️ *[Bạn điền: ví dụ người ngồi trên xe máy tách thành 1 `person` + 1 `motorcycle`; ảnh này có 20 person và 6 motorcycle.]*

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: ✏️ *[Bạn điền]*
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: ✏️
- Bằng chứng tôi nhìn thấy: ✏️
- Quy tắc và hành động sửa: ✏️
- Sau sửa đã Save và export lại chưa? ✏️

Chưa xem Summary tự đánh giá trên GitHub Actions / chưa có điểm.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 · `cp5_occlusion` `000000336232.jpg`, xe `car` lớn ở phía trên ảnh (bbox x≈299–527, y≈3–126), vẽ bằng Brush | (a) Một xe bị vật khác che thành nhiều mảng là **một** instance; (b) mỗi mảng nhìn thấy là một object riêng | Quy tắc occlusion: vật bị che vẫn là một mask, chỉ vẽ phần nhìn thấy | Giữ là **1 object** (một mask Brush). ✏️ *[Bạn xác nhận / bổ sung vật che là gì]* |
| 2 · `cp1_holes` `000000144300.jpg`, xe `car` nhỏ bên phải (bbox x≈574–624, y≈143–179) | (a) Cửa kính là lỗ, khoét khỏi mask; (b) kính là một phần của xe, giữ trong mask | Quy tắc holes: kính/khe nằm trong mask vật, không khoét | Giữ kính trong mask xe. ✏️ *[Bạn xác nhận]* |
| 3 · ✏️ *[Bạn điền, gợi ý: `cp4_curb`, sân bê tông chỗ SUV trắng bên phải và dải cỏ cạnh bó vỉa]* | (a) sidewalk vì cùng bê tông, liền lối đi; (b) không phải lối đi bộ → để trống | Ranh road–sidewalk theo chức năng, không theo chất liệu | ✏️ *[Bạn đã tô thế nào?]* |
