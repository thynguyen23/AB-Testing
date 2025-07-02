
# A/B Testing: Phân tích tác động của việc lọc học viên theo cam kết thời gian học

## 1. Mục tiêu thí nghiệm
Mục tiêu của thí nghiệm là kiểm tra xem việc hỏi học viên về thời gian họ có thể dành cho khóa học trước khi ghi danh có giúp:
- Cải thiện trải nghiệm học tập của học viên.
- Giảm tỷ lệ học viên bỏ học sớm (huỷ trước 14 ngày).
- Tối ưu hóa tài nguyên hỗ trợ (coaching).

## 2. Thiết kế thí nghiệm
- **Nhóm kiểm soát:** Học viên được ghi danh vào dùng thử miễn phí như bình thường.
- **Nhóm thí nghiệm:** Sau khi nhấn “Dùng thử miễn phí”, học viên được hỏi: "Bạn có thể dành bao nhiêu thời gian mỗi tuần cho khoá học?".
  - Nếu chọn từ 5h trở lên: tiếp tục ghi danh.
  - Nếu chọn dưới 5h: hiển thị cảnh báo khuyến nghị sử dụng phiên bản miễn phí (không có hỗ trợ và chứng chỉ).
- **Đơn vị phân nhóm:** Cookie (trước khi ghi danh), user-id (sau ghi danh).
- **Thời gian thí nghiệm:** 23 ngày với dữ liệu Enrollment, 39 ngày với dữ liệu Pageview và Click.

## 3. Các chỉ số chính

### Invariant Metrics (dự kiến không thay đổi giữa 2 nhóm)
- Số lượng cookie (truy cập vào trang).
- Số lượt nhấn “Dùng thử miễn phí”.
- Click-Through Probability: Click / Pageviews.

Tất cả đều “vượt qua” kiểm định thống kê và không có sai lệch giữa hai nhóm.

### Evaluation Metrics (cần theo dõi thay đổi):
- **Gross Conversion (GC):** Enrollments / Clicks. Mức chấp nhận thay đổi: 1%.
- **Retention:** Payments / Enrollments. Mức chấp nhận thay đổi: 1%.
- **Net Conversion (NC):** Payments / Clicks. Mức chấp nhận thay đổi: 0.75%.

## 4. Kết quả

| Metric            | Mức chênh lệch quan sát | Khoảng tin cậy (95%)        | Kết luận                                 |
|-------------------|--------------------------|------------------------------|-------------------------------------------|
| Gross Conversion  | -2.06%                   | [-2.92%, -1.20%]             | Có ý nghĩa thống kê & thực tiễn          |
| Net Conversion    | -0.49%                   | [-1.16%, 0.19%]              | Không có ý nghĩa thống kê                |

## 5. Kết luận & Khuyến nghị
Mặc dù tỷ lệ ghi danh có giảm đáng kể, nhưng không có bằng chứng cho thấy chất lượng học viên (tỷ lệ trả phí sau 14 ngày) tăng lên. Do đó:
- **Không nên triển khai chính thức thay đổi này.**
- Cần thử nghiệm thêm các hình thức lọc học viên khác.

## 6. Đề xuất thử nghiệm tiếp theo

### 6.1 Can thiệp trước ghi danh (Pre-enrollment)
- Bổ sung thêm danh sách kỹ năng nền tảng bên cạnh câu hỏi về thời gian.
- Nếu học viên đáp ứng cả thời gian và kỹ năng → được ghi danh dùng thử.
- Nếu không → khuyến khích học miễn phí.
- Mục tiêu: giảm Gross Conversion nhưng tăng Net Conversion.

### 6.2 Can thiệp sau ghi danh (Post-enrollment)
- Gợi ý học viên tham gia nhóm học (team) ngay sau khi ghi danh.
- Thiết lập nhóm thí nghiệm và nhóm kiểm soát.
- Chỉ số đánh giá: Retention (tỷ lệ tiếp tục học sau 14 ngày).
- Mục tiêu: tăng Retention mà không cần tăng hỗ trợ từ huấn luyện viên.

## 7. Tổng kết
Thí nghiệm đã được thiết kế bài bản, sử dụng các phương pháp kiểm định thống kê, kiểm tra phương sai và Sign Test. Tuy nhiên, mục tiêu cuối cùng - **tăng tỷ lệ học viên chất lượng** - chưa đạt được. Vì vậy cần tiếp tục thử nghiệm các giải pháp thay thế như bổ sung điều kiện đầu vào hoặc hỗ trợ sau ghi danh một cách tối ưu hơn.
