# Báo cáo thực hành Day 3

- Mã học viên theo lớp: 2A202603164
- Ngày / CVAT local: 11–12/11/2026 / CVAT local v2.74.1 (Docker Desktop + WSL)
- Công cụ đã dùng: CVAT Polygon, Brush và SAM ViT-B. Tôi dùng SAM ở medium_instance, hard_panoptic và các checkpoint; easy_semantic thì vẽ tay.

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | submissions/easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | submissions/medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | submissions/hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | submissions/cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | submissions/cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | submissions/cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | submissions/cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | submissions/cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | submissions/cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Tôi đã chạy script kiểm tra và cả 9 file đều báo OK về cấu trúc.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên: người đứng ở khoảng giữa bên trái của ảnh 1. Tôi dùng SAM ngay từ object này, nên chưa làm đúng yêu cầu tự vẽ object đầu trước khi xem gợi ý.
- Class và quy tắc tôi dùng để chọn biên: person. Khi kiểm mask SAM, tôi giữ biên ở mép phần cơ thể nhìn thấy, không để lấn sang nền hay người bên cạnh, và không vẽ phần bị che.
- Gợi ý SAM sai ở đâu và tôi đã xử lý thế nào: SAM hay ăn lẹm sang nền và bóng đổ dưới chân người, dưới gầm xe, tôi xóa phần tràn bằng Eraser. Nó cũng hay bỏ sót chi tiết mảnh như tay, chân, gương xe, bánh xe, tôi tô bù bằng Brush hoặc thêm điểm vào polygon. Với các object còn lại, tôi kiểm lại class và số lượng trong danh sách Objects trước khi giữ.
- Quyết định gán nhãn của tôi: người ngồi trên xe máy và chiếc xe máy tôi tách thành hai object (person và motorcycle), không gộp chung một mask. Ảnh 1 có nhiều xe máy nên tôi phải tách khá nhiều cặp như vậy.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: medium_instance, ảnh 3, toàn bộ ảnh.
- Lỗi thuộc loại: thiếu vật.
- Bằng chứng tôi nhìn thấy: khi kiểm lại file export, ảnh 1 và ảnh 2 đều có object, còn ảnh 3 hoàn toàn trống, trong khi ảnh có khá nhiều người và xe đỗ hai bên đường.
- Quy tắc và hành động sửa: mỗi vật nhìn thấy phải có mask riêng, nên ảnh trống là thiếu. Tôi mở lại task, vẽ bổ sung người và xe trong ảnh 3, mỗi vật một object, rồi xem lại danh sách Objects.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại COCO 1.0, file mới có đủ 3 ảnh và script kiểm tra báo OK.

Chưa xem Summary tự đánh giá trên GitHub Actions / chưa có điểm.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 · cp5_occlusion, dòng xe ở xa phía sau xe buýt | (a) Tách từng xe thành object riêng; (b) xe quá nhỏ và dính sát nhau, không thấy rõ ranh từng chiếc, nên để chung một mask | Chỉ vẽ phần nhìn thấy; ở khoảng cách đó tôi không phân biệt được biên từng xe, tách ra thì chỉ là đoán | Tôi để chung một mask car; các xe ở gần thì vẫn tách riêng từng chiếc. Câu hỏi cho coach: cụm xe ở xa như vậy nên tách hay để chung? |
| 2 · medium_instance ảnh 3, người ngồi xe lăn trong nhóm người qua đường bên trái | (a) Xe lăn gắn liền với người nên gộp vào mask person; (b) xe lăn không thuộc class nào của task, nên chỉ lấy phần người, xe lăn để làm nền | Chỉ dùng class có trong classes.json. Người và xe lăn ở rất xa, nhỏ và sát người bên cạnh nên khó tách biên | Tôi để một mask person bao cả người và xe lăn, vì ở kích thước đó rất khó tách ranh. Câu hỏi cho coach: vật không có class như xe lăn nên gộp vào person hay bỏ ra ngoài? |
| 3 · cp4_curb, sân bê tông bên phải chỗ xe SUV trắng đỗ và dải cỏ cạnh bó vỉa | Sân bê tông: (a) sidewalk vì cùng mặt lát và nối liền lối đi bộ; (b) là chỗ đỗ xe nên để trống. Dải cỏ: (a) gộp vào sidewalk; (b) không phải lối đi nên để trống | Ranh road và sidewalk xét theo chức năng và bó vỉa, không theo màu | Tôi tô sân bê tông là sidewalk vì nó nối liền lối đi bộ; dải cỏ và bó vỉa để trống; road dừng ở chân bó vỉa. Đoạn vỉa hè mảnh ở xa bên trái tôi chưa tô. Câu hỏi cho coach: vùng xa và mảnh như vậy có cần tô không? |
