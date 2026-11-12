# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn
- Mã học viên theo lớp: 2A202603291
- Ngày / CVAT local: 12/11/2026 — http://localhost:8080
- Công cụ đã dùng: CVAT, SAM, Polygon, Brush.
## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh đầu tiên trong medium_instance, đối tượng xe máy ở khu vực giữa ảnh.
- Class và quy tắc tôi dùng để chọn biên: Class motorcycle. Tôi xác định phần thân xe, bánh xe và các chi tiết thuộc xe, vẽ theo đường viền quan sát được, không lấy phần nền phía sau hoặc các đối tượng khác vào mask.
- Nếu dùng gợi ý sau đó: Tôi sử dụng SAM để hỗ trợ phân đoạn. Một số vùng gần bánh xe và các chi tiết nhỏ có thể bị chọn thiếu hoặc lấn sang nền. Tôi kiểm tra và chỉnh sửa lại mask để bám sát đường viền đối tượng hơn.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: cp1_holes — ảnh xe máy, vùng khoảng trống giữa các bộ phận xe.
- Lỗi thuộc loại: Biên / phủ vùng.
- Bằng chứng tôi nhìn thấy: Khi kiểm tra mask, một số khoảng trống giữa các chi tiết xe bị tô kín, dù có thể nhìn thấy phần nền phía sau.
- Quy tắc và hành động sửa: Tôi giữ mask ở những phần thực sự thuộc đối tượng, loại bỏ các vùng nền bị chọn thừa và chỉnh lại đường biên quanh các khoảng trống.
- Sau sửa đã Save và export lại chưa? Đã Save và export ZIP sau khi hoàn thành chỉnh sửa.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Chưa chạy tự đánh giá / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| cp1_holes — khoảng trống giữa các bộ phận xe máy | Tô kín toàn bộ vùng bên trong đường bao hoặc giữ các lỗ trống | Có thể quan sát thấy nền phía sau qua các khoảng trống, chứng tỏ chúng không thuộc đối tượng | Giữ lại các lỗ trống, chỉ tô mask lên những bộ phận thực sự thuộc xe |
| cp5_occlusion — vùng đối tượng bị che khuất | Vẽ cả phần khuất hoặc chỉ phần nhìn thấy | Phần bị che không có đường biên trực tiếp quan sát được, nếu vẽ thêm sẽ phải suy đoán | Chỉ phân đoạn phần nhìn thấy rõ, tránh tự thêm vùng không có bằng chứng |
| cp3_thin — các chi tiết mảnh ở rìa đối tượng | Bỏ qua chi tiết quá nhỏ hoặc cố gắng giữ lại | Khi phóng to vẫn nhận thấy chi tiết mảnh nối liền với đối tượng | Giữ lại các chi tiết nhìn thấy được, điều chỉnh đường biên để tránh làm mất phần mảnh hoặc lấy thừa nền |

