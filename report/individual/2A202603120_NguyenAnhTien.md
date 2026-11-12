# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603120
- Ngày / CVAT local: 12/11/2026 / CVAT local (`localhost:8080`)
- Công cụ đã dùng: CVAT local; YOLO và SAM3 làm gợi ý; Codex hỗ trợ thao tác/kiểm tra (; `scoring/score.py` để tự đánh giá sau khi nhận gold.

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `medium_instance`, ảnh 1 (`000000181542.jpg`), người nhỏ ở gần giữa phía trên, sát vùng xe tải (xấp xỉ x=363, y=122 trong ảnh 640×480); tôi vẽ lại từ đầu trước khi mở gợi ý.
- Class và quy tắc tôi dùng để chọn biên: `person`; theo phần viền nhìn thấy và dừng ở ranh bị xe che, không đoán phần khuất.
- Nếu dùng gợi ý sau đó: YOLO và SAM3 chỉ để tham khảo; tôi tự kiểm tra từng mask theo ảnh rồi quyết định giữ hoặc sửa theo ranh nhìn thấy.
- Nếu không dùng gợi ý: không áp dụng; tôi có dùng YOLO và SAM3 làm tham khảo, còn quyết định cuối cùng do tôi tự kiểm tra.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `easy_semantic`, ảnh 1, vùng vegetation sát ranh road và các xe nhỏ ở xa.
- Lỗi thuộc loại: thiếu vùng vegetation nhìn thấy và mask road lấn vào vùng xe.
- Bằng chứng tôi nhìn thấy: có các chấm xanh rải rác gần road và sky; các xe nhỏ ở xa vẫn có đường bao nhìn thấy nên không nên gộp vào road.
- Quy tắc và hành động sửa: trong CVAT, tôi điều chỉnh mask vegetation/road theo vùng thực nhìn thấy và giữ ranh xe nhỏ tách khỏi road.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có):

- Lần chạy đầu (đã xem gold, trước các sửa CVAT): `easy_semantic` mIoU 0.803 (17.9/20; coverage 95.3%; sidewalk IoU 0.564); `medium_instance` 0.624 (16/32; mean matched IoU 0.778, P@0.5 0.78, R@0.5 0.80, TP 57, FP 16, FN 14; 73 dự đoán so với 71 GT); `hard_panoptic` PQ 0.443 (16.2/30; cần rà person, sidewalk và car). Tổng ba tier 50.1/82, chưa gồm 18 điểm checkpoint.
- Sau khi sửa trong CVAT và export lại: `easy_semantic` mIoU 0.991 (20/20), `medium_instance` 0.994 (32/32), `hard_panoptic` PQ 0.873 (30/30); tổng tự đánh giá ba tier 82/82. Scorecard có `REVIEW_HIGH_AGREEMENT` ở Easy và Medium; đây là tín hiệu để coach kiểm tra reference/provenance, không phải kết luận hay điểm chính thức.

Các mức điểm trên là tự đánh giá sau khi xem gold, không phải điểm chính thức. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `medium_instance`, ảnh 1 (`000000181542.jpg`), taxi vàng ở trước bus đỏ | Tách mask theo từng xe và chỉ lấy phần nhìn thấy / gộp vùng chồng hoặc vẽ tiếp phần bị che | Hai xe là hai object khác class; ranh chỉ quan sát được ở phần lộ ra | Hỏi coach: với xe bị che, có nên tách từng object và dừng mask ở ranh bị che không? |
| `medium_instance`, ảnh 1, các xe nhỏ ở cuối đường | Gán nhãn xe nhỏ nếu còn nhận ra / bỏ qua nếu quá nhỏ hoặc không rõ | Xe ở xa có ít chi tiết; khó xác định biên mask | Hỏi coach ngưỡng kích thước/độ rõ để vẫn phải gán nhãn xe ở xa. |
| `medium_instance`, ảnh 2 (`000000373353.jpg`), nhóm người ở xa gần giao lộ | Gán từng người nhìn thấy / bỏ người nếu quá mờ hoặc lẫn vào đám đông | Một số người nhỏ và sát nhau nên biên giữa các instance khó nhìn | Hỏi coach mức độ rõ cần có để gán riêng từng người ở xa. |
