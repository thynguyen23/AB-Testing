# 📊 Tóm Tắt Dự Án A/B Testing: Ảnh Hưởng của Bộ Lọc Thời Gian Cam Kết

## 🎯 Mục Tiêu
Đánh giá xem việc thêm một câu hỏi về **cam kết thời gian học tập** trước khi cho dùng thử miễn phí có ảnh hưởng gì đến:

- **Gross Conversion**: Tỷ lệ từ click sang đăng ký học thử
- **Net Conversion**: Tỷ lệ từ click sang trả phí sau 14 ngày dùng thử

Mục tiêu là **tăng tỷ lệ học viên trả phí** mà **giảm số người bỏ sớm**.

---

## ⚙️ Thiết Lập Thử Nghiệm

- **Đơn vị phân nhóm (Unit of Diversion):** Cookie (mỗi người dùng theo ID cookie)
- **Đơn vị phân tích (Unit of Analysis):** Clicks
- **Thời gian chạy thử nghiệm:**
  - Pageviews/Clicks: 39 ngày
  - Enrollments/Payments: 23 ngày (dữ liệu bị giới hạn)
- **Giả thuyết:**
  - H₀ (Gross): Không có sự khác biệt đáng kể giữa 2 nhóm
  - H₀ (Net): Không có sự khác biệt đáng kể giữa 2 nhóm

---

## ✅ Kiểm Tra Tính Hợp Lý (Sanity Checks)

| Chỉ số                                  | Giá trị kỳ vọng | Quan sát được | CI thấp | CI cao  | Kết luận |
|-----------------------------------------|-----------------|---------------|---------|---------|----------|
| Số lượng Cookies                        | 0.5000          | 0.5006        | 0.4988  | 0.5012  | Pass     |
| Click vào “Start Free Trial”           | 0.5000          | 0.5005        | 0.4959  | 0.5042  | Pass     |
| Tỷ lệ Click-through (CTP)              | 0.0821          | 0.0822        | 0.0812  | 0.0830  | Pass     |

✅ Các nhóm được phân chia đồng đều → dữ liệu có thể dùng cho phân tích.

---

## 📈 Phân Tích Chỉ Số Đánh Giá (Evaluation Metrics)

### Gross Conversion (Click → Đăng ký học thử)
- **Khác biệt quan sát:** -0.0205
- **Khoảng tin cậy 95%:** [-0.0291, -0.0120]
- **Ngưỡng dmin:** 0.01
- ✅ **Có ý nghĩa thống kê và thực tiễn**

### Net Conversion (Click → Trả phí)
- **Khác biệt quan sát:** -0.0048
- **Khoảng tin cậy 95%:** [-0.0116, 0.0019]
- **Ngưỡng dmin:** 0.0075
- ❌ **Không có ý nghĩa thống kê và thực tiễn**

---

## 🧪 Kiểm Định Dấu Hiệu (Sign Test)

| Chỉ số           | p-value | Có ý nghĩa thống kê ở α = 0.05? |
|------------------|---------|---------------------------------|
| Gross Conversion | 0.0026  | ✅ Có                            |
| Net Conversion   | 0.6776  | ❌ Không                         |

✅ Gross Conversion giảm đồng đều theo ngày  
❌ Net Conversion không có xu hướng rõ ràng

---

## 📌 Kết Luận Chính

> “Bản cập nhật gây giảm tỷ lệ đăng ký học thử (Gross Conversion), **nhưng không tăng tỷ lệ trả phí (Net Conversion)** → Không đạt mục tiêu kinh doanh.”

---

## 🚫 Khuyến Nghị

**KHÔNG TRIỂN KHAI THAY ĐỔI.**
- Mặc dù có tác động rõ ràng đến Gross Conversion, thay đổi không giúp tăng số người trả phí.
- Có nguy cơ làm giảm số học viên đăng ký → không có lợi ích thực tiễn.

---

## 🔁 Đề Xuất Thí Nghiệm Tiếp Theo

### 1. Can Thiệp Trước Khi Ghi Danh (Pre-enrollment Intervention)
- 👉 Thêm checklist các kỹ năng nền tảng vào pop-up hỏi thời gian.
- ✅ Chỉ học viên đạt đủ thời gian & kỹ năng mới được chuyển đến trang ghi danh.
- Kết quả mong đợi: Gross Conversion giảm nhưng Net Conversion tăng.

### 2. Can Thiệp Sau Khi Ghi Danh (Post-enrollment Intervention)
- 👉 Tạo nhóm học tập (study team) để hỗ trợ học viên sau khi đăng ký.
- **Thiết kế thử nghiệm:**
  - **Đơn vị phân nhóm:** `user-id`
  - **Chỉ số đánh giá:** Tỷ lệ Retention sau 14 ngày.
- ✅ Nếu Retention tăng có ý nghĩa thống kê và không tiêu tốn nhiều tài nguyên → triển khai.

---



