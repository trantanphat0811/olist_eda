# 📚 Theory — Olist E-Commerce Sales Analysis

> Tài liệu lý thuyết nền tảng cho dự án phân tích dữ liệu thương mại điện tử Olist.

---

## 1. Exploratory Data Analysis (EDA) là gì?

**EDA (Phân tích Dữ liệu Khám phá)** là bước đầu tiên và quan trọng nhất trong bất kỳ dự án Data Analytics nào. Mục tiêu là **hiểu dữ liệu trước khi phân tích sâu**.

### Các bước EDA cơ bản:
```
1. Load dữ liệu → kiểm tra shape, columns, dtypes
2. Kiểm tra Missing Values → quyết định xử lý
3. Kiểm tra Duplicates → loại bỏ nếu cần
4. Phân tích thống kê mô tả → mean, median, std, min, max
5. Visualization → tìm pattern, trend, outlier
6. Kết luận insight → đưa ra nhận xét business
```

### Tại sao EDA quan trọng?
- Phát hiện dữ liệu bị lỗi, thiếu hoặc bất thường trước khi phân tích
- Hiểu phân phối và mối quan hệ giữa các biến
- Định hướng câu hỏi business cần trả lời
- Tránh đưa ra kết luận sai do dữ liệu kém chất lượng

---

## 2. Các kỹ thuật xử lý dữ liệu

### 2.1 Missing Values (Dữ liệu bị thiếu)

**Nguyên nhân phổ biến:**
- Người dùng không điền đầy đủ thông tin
- Lỗi kỹ thuật trong quá trình thu thập
- Dữ liệu không áp dụng cho trường hợp đó

**Cách xử lý:**
```python
# Kiểm tra missing values
df.isnull().sum()

# Xóa dòng có missing values ở cột quan trọng
df.dropna(subset=['Customer ID'])

# Điền giá trị thay thế (mean/median/mode)
df['column'].fillna(df['column'].mean())
```

**Trong dự án Olist:**
- `Customer ID`: 22.77% missing → **xóa** (không thể phân tích khách hàng)
- `Description`: 0.41% missing → **bỏ qua** (không ảnh hưởng phân tích)

---

### 2.2 Data Type Conversion (Chuyển đổi kiểu dữ liệu)

```python
# Chuyển string → datetime
df['order_purchase_timestamp'] = pd.to_datetime(df['order_purchase_timestamp'])

# Trích xuất thông tin từ datetime
df['year']       = df['date'].dt.year
df['month']      = df['date'].dt.month
df['year_month'] = df['date'].dt.to_period('M').astype(str)
df['weekday']    = df['date'].dt.day_name()
```

**Tại sao quan trọng:**
- Python mặc định đọc ngày tháng dưới dạng string
- Cần chuyển sang datetime để tính toán khoảng thời gian
- Mới có thể group by tháng, quý, năm

---

### 2.3 Data Merging (Gộp bảng dữ liệu)

**Các loại JOIN:**

| Loại | Ý nghĩa | Dùng khi |
|---|---|---|
| `INNER JOIN` | Chỉ giữ dòng khớp ở cả 2 bảng | Muốn dữ liệu đầy đủ 100% |
| `LEFT JOIN` | Giữ tất cả dòng bảng trái | Bảng chính + thông tin bổ sung |
| `RIGHT JOIN` | Giữ tất cả dòng bảng phải | Hiếm dùng |
| `OUTER JOIN` | Giữ tất cả dòng cả 2 bảng | Muốn không mất dữ liệu nào |

```python
# Merge trong Pandas
df = orders.merge(items,     on='order_id',   how='left')
           .merge(payments,  on='order_id',   how='left')
           .merge(customers, on='customer_id', how='left')
```

**Trong dự án Olist:** Dùng **LEFT JOIN** vì `orders` là bảng chính, các bảng còn lại bổ sung thông tin.

---

## 3. Visualization — Các loại biểu đồ

### 3.1 Line Chart (Biểu đồ đường)
**Dùng để:** Hiển thị xu hướng theo thời gian

```python
plt.plot(x, y, marker='o', linewidth=2)
```

**Khi nào dùng:**
- Doanh thu theo tháng/quý/năm
- Số lượng đơn hàng theo thời gian
- Bất kỳ dữ liệu có thứ tự thời gian

**Đọc biểu đồ:**
- Đường đi lên → tăng trưởng tốt
- Đường đi xuống → suy giảm cần chú ý
- Đỉnh nhọn → sự kiện đặc biệt (sale, lễ)

---

### 3.2 Bar Chart (Biểu đồ cột)
**Dùng để:** So sánh giá trị giữa các nhóm

```python
# Cột đứng
plt.bar(x, height)

# Cột nằm ngang (tốt hơn khi label dài)
plt.barh(y, width)
```

**Khi nào dùng:**
- So sánh doanh thu theo bang/quốc gia
- Top N sản phẩm/danh mục bán chạy
- Phân bổ trạng thái đơn hàng

---

### 3.3 Pie/Donut Chart (Biểu đồ tròn)
**Dùng để:** Hiển thị tỉ lệ phần trăm của từng phần trong tổng thể

**Khi nào dùng:** Khi có ít hơn 6-7 phần (nhiều hơn sẽ khó đọc)

**Hạn chế:** Khó so sánh chính xác → nên dùng kèm Bar Chart

---

### 3.4 Histogram (Biểu đồ tần suất)
**Dùng để:** Hiển thị phân phối của một biến liên tục

```python
plt.hist(data, bins=50)
```

**Đọc histogram:**
- Phân phối chuẩn → hình chuông đối xứng
- Lệch phải → nhiều giá trị nhỏ, ít giá trị lớn
- Outlier → cột lẻ ở xa phần còn lại

**Trong dự án:** Phân phối `delivery_days` → thấy đa số giao 5-20 ngày, một số outlier giao >40 ngày

---

### 3.5 Dual-axis Chart (Biểu đồ 2 trục Y)
**Dùng để:** Hiển thị 2 chỉ số có đơn vị khác nhau trên cùng 1 chart

```python
fig, ax1 = plt.subplots()
ax1.bar(x, orders)      # Trục Y trái — số đơn hàng
ax2 = ax1.twinx()
ax2.plot(x, revenue)    # Trục Y phải — doanh thu
```

**Ứng dụng trong Olist:** Vừa thấy số đơn (cột) vừa thấy doanh thu (đường) theo tháng

---

## 4. Business Metrics (Chỉ số kinh doanh)

### 4.1 Revenue (Doanh thu)
```
Revenue = Quantity × Unit Price
```
- Chỉ số quan trọng nhất đo lường hiệu quả kinh doanh
- Phân tích theo thời gian để thấy xu hướng tăng/giảm
- Phân tích theo địa lý để tối ưu thị trường

### 4.2 Cancellation Rate (Tỉ lệ hủy đơn)
```
Cancel Rate = Số đơn hủy / Tổng số đơn × 100%
```
- Tỉ lệ cao → vấn đề về trải nghiệm khách hàng, hết hàng, hoặc giá cả
- Olist: ~0.6% cancel rate → khá thấp và tốt

### 4.3 Average Order Value (Giá trị đơn hàng trung bình)
```
AOV = Tổng doanh thu / Số đơn hàng
```
- Thể hiện mức chi tiêu trung bình mỗi lần mua
- Tăng AOV → khuyến khích mua thêm, bundle deals

### 4.4 Review Score (Điểm đánh giá)
- Thang điểm 1-5 từ khách hàng sau khi nhận hàng
- Tương quan nghịch với delivery time: giao chậm → review thấp
- **Insight quan trọng:** Giao hàng nhanh là yếu tố then chốt tạo ra review tốt

---

## 5. Power BI — Các khái niệm cơ bản

### 5.1 DAX (Data Analysis Expressions)
Ngôn ngữ công thức trong Power BI để tính toán:

```dax
-- Tổng doanh thu
Total Revenue = SUM(table[payment_value])

-- Đếm số lượng duy nhất
Total Orders = DISTINCTCOUNT(table[order_id])

-- Tỉ lệ phần trăm có điều kiện
Cancel Rate % = 
DIVIDE(
    CALCULATE(COUNTROWS(table), table[status] = "canceled"),
    COUNTROWS(table)
) * 100
```

### 5.2 Các loại Visual trong Power BI

| Visual | Dùng để |
|---|---|
| Card | Hiển thị 1 chỉ số KPI quan trọng |
| Line Chart | Xu hướng theo thời gian |
| Bar/Column Chart | So sánh giữa các nhóm |
| Donut/Pie Chart | Tỉ lệ phần trăm |
| Table | Dữ liệu chi tiết dạng bảng |
| Scatter Chart | Mối quan hệ giữa 2 biến số |
| Map | Phân bố địa lý |

### 5.3 Filter trong Power BI
- **Top N Filter:** Lọc N phần tử lớn nhất/nhỏ nhất
- **Relative Date Filter:** Lọc theo khoảng thời gian tương đối
- **Cross-filtering:** Click vào visual này → tự động filter các visual khác

---

## 6. Insights & Storytelling với Data

### 6.1 Pyramid Insight
Khi trình bày insight, đi từ **tổng quát → cụ thể**:

```
Tổng doanh thu: 19.87M BRL
  → Bang SP chiếm 40%
    → Category bed_bath_table dẫn đầu tại SP
      → Peak tháng 11 do mùa lễ hội
```

### 6.2 So sánh & Benchmark
Luôn đặt số liệu trong bối cảnh so sánh:
- **Tốt hay xấu?** → So với trung bình ngành
- **Tăng hay giảm?** → So với kỳ trước
- **Cao hay thấp?** → So với các nhóm khác

### 6.3 Actionable Insights (Insight có thể hành động)

❌ **Insight yếu:** "Doanh thu tháng 11 cao hơn các tháng khác"

✅ **Insight mạnh:** "Doanh thu tháng 11 cao hơn 40% so với trung bình — nên tăng tồn kho và nhân lực giao hàng vào Q4 hàng năm"

---

## 7. Quy trình Data Analytics chuẩn

```
1. DEFINE    → Xác định câu hỏi business cần trả lời
      ↓
2. COLLECT   → Thu thập dữ liệu từ các nguồn
      ↓
3. CLEAN     → Xử lý missing, outlier, duplicate
      ↓
4. ANALYZE   → EDA, tính toán metrics, visualization
      ↓
5. INTERPRET → Rút ra insights từ phân tích
      ↓
6. PRESENT   → Trình bày kết quả cho stakeholder
      ↓
7. ACT       → Đề xuất hành động dựa trên data
```

---

## 8. Câu hỏi phỏng vấn thường gặp

**Q: EDA gồm những bước nào?**
A: Load data → kiểm tra shape/dtypes → missing values → duplicates → thống kê mô tả → visualization → insight.

**Q: Khi gặp missing values, bạn xử lý thế nào?**
A: Phụ thuộc vào tỉ lệ và tầm quan trọng của cột. Nếu <5% và không quan trọng → drop. Nếu quan trọng → impute bằng mean/median/mode hoặc dùng model để predict.

**Q: LEFT JOIN và INNER JOIN khác nhau thế nào?**
A: LEFT JOIN giữ tất cả dòng của bảng trái dù không có match. INNER JOIN chỉ giữ dòng có match ở cả 2 bảng.

**Q: Bạn chọn loại chart nào để hiển thị xu hướng theo thời gian?**
A: Line Chart — vì thể hiện rõ sự liên tục và xu hướng tăng/giảm theo thời gian.

**Q: DAX là gì?**
A: Data Analysis Expressions — ngôn ngữ công thức trong Power BI dùng để tạo calculated columns và measures cho phân tích dữ liệu.

---

*Tài liệu này được biên soạn dựa trên thực tế thực hiện dự án Olist E-Commerce Sales Analysis.*
