
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
- ### Số Lượng Mẫu và Mức Độ Power
Số lượng pageviews cần thiết cho từng chỉ số được tính toán với **giá trị alpha = 0.05** và **beta = 0.2** (tương ứng với power = 80%).

---

### Số Lượng Pageviews Cần Thiết Cho Mỗi Chỉ Số Để Đạt Power Mong Muốn

#### **1. Gross Conversion**
- **Tỷ lệ chuyển đổi gốc (Baseline Conversion):** 20.625%  
- **Mức ảnh hưởng nhỏ nhất có thể phát hiện (Minimum Detectable Effect):** 1%  
- **Alpha:** 5%  
- **Beta:** 20%  
- **Power:** 80%  
- **Kích thước mẫu:** 25,835 lượt ghi danh / nhóm  
- **Số nhóm:** 2 (Thử nghiệm và Đối chứng)  
- **Tổng số mẫu:** 51,670 lượt ghi danh  
- **Số lần click / pageview:** 3,200 / 40,000 = 0.08  
- **Số pageviews cần thiết:** 645,875

---

#### **2. Retention**
- **Tỷ lệ giữ chân gốc (Baseline Conversion):** 53%  
- **Minimum Detectable Effect:** 1%  
- **Alpha:** 5%  
- **Beta:** 20%  
- **Power:** 80%  
- **Kích thước mẫu:** 39,155 lượt ghi danh / nhóm  
- **Số nhóm:** 2 (Thử nghiệm và Đối chứng)  
- **Tổng số mẫu:** 78,230 lượt ghi danh  
- **Số lượt ghi danh / pageview:** 660 / 40,000 = 0.0165  
- **Số pageviews cần thiết:** 78,230 / 0.0165 = 4,741,212

---

#### **3. Net Conversion**
- **Tỷ lệ chuyển đổi gốc:** 10.9313%  
- **Minimum Detectable Effect:** 0.75%  
- **Alpha:** 5%  
- **Beta:** 20%  
- **Power:** 80%  
- **Kích thước mẫu:** 27,413 lượt ghi danh / nhóm  
- **Số nhóm:** 2 (Thử nghiệm và Đối chứng)  
- **Tổng số mẫu:** 54,826  
- **Số lần click / pageview:** 3,200 / 40,000 = 0.08  
- **Số pageviews cần thiết:** 685,325

Không xét Retention do:
Yêu cầu tới ~4.7 triệu pageviews để đạt power 80%.
Tốc độ traffic 40.000 pageviews/ngày → mất 119 ngày chạy.
## 4. Kết quả
### 4.1 Sanity Check (Invariate Metrics)

| Metric  | Giá trị đo được | CI                        | Kết quả |
|---------|------------------|----------------------------|---------|
| Cookies | 0.5006           | [0.4988, 0.5012]           | Pass    |
| Clicks  | 0.5005           | [0.4959, 0.5042]           | Pass    |
| CTP     | 0.0822           | [0.0812, 0.0830]           | Pass    |

---

### 4.2 Evaluation Metrics

| Metric           | Dmin   | Chênh lệch đo được | CI                         | Kết luận                              |
|------------------|--------|---------------------|-----------------------------|----------------------------------------|
| Gross Conversion | 0.01   | -0.0205             | [-0.0291, -0.0120]          | Đáng kể về cả thực lẫn thống kê       |
| Net Conversion   | 0.0075 | -0.0048             | [-0.0116, 0.0019]           | KHÔNG đáng kể                         |

---

### 4.3 Kiểm Định Sign Test

| Metric           | p-value | Đáng kể về thống kê? |
|------------------|---------|-----------------------|
| Gross Conversion | 0.0026  | Có                    |
| Net Conversion   | 0.6776  | KHÔNG                 |

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
