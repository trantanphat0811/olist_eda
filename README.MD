# 🛒 Olist E-Commerce Sales Analysis

> Phân tích toàn diện dữ liệu thương mại điện tử Brazil giai đoạn 2016–2018 sử dụng Python và Power BI.

---

## 📌 Giới thiệu dự án

Dự án này phân tích bộ dữ liệu thực tế từ **Olist** — nền tảng thương mại điện tử lớn nhất Brazil — với hơn **96,000 đơn hàng** trong giai đoạn 2016–2018. Mục tiêu là tìm ra các insight có giá trị về doanh thu, hành vi khách hàng, hiệu suất giao hàng và mức độ hài lòng của người mua.

---

## 📊 Dataset

- **Nguồn:** [Kaggle — Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Thời gian:** Tháng 10/2016 – Tháng 8/2018
- **Quy mô:** 9 bảng dữ liệu liên kết, ~99,000 đơn hàng

---

## 🎯 Mục tiêu phân tích

- Phân tích xu hướng doanh thu theo thời gian
- Xác định các bang và danh mục sản phẩm có hiệu suất cao nhất
- Đánh giá mối quan hệ giữa thời gian giao hàng và sự hài lòng của khách hàng
- Phân tích phương thức thanh toán phổ biến
- Xây dựng dashboard trực quan phục vụ ra quyết định kinh doanh

---

## 🛠️ Công cụ & Thư viện

| Công cụ | Mục đích |
|---|---|
| Python 3.10 | Xử lý và phân tích dữ liệu |
| Pandas | Load, merge, làm sạch dữ liệu |
| NumPy | Tính toán số học |
| Matplotlib | Vẽ biểu đồ cơ bản |
| Seaborn | Visualization nâng cao |
| Power BI | Dashboard tương tác |
| VS Code + Jupyter | Môi trường phát triển |

---

## 📁 Cấu trúc dự án

```
olist-eda/
├── data/
│   ├── olist_orders_dataset.csv
│   ├── olist_order_items_dataset.csv
│   ├── olist_order_payments_dataset.csv
│   ├── olist_order_reviews_dataset.csv
│   ├── olist_customers_dataset.csv
│   ├── olist_sellers_dataset.csv
│   ├── olist_products_dataset.csv
│   ├── olist_geolocation_dataset.csv
│   ├── product_category_name_translation.csv
│   ├── powerbi_main.csv
│   ├── powerbi_monthly.csv
│   ├── powerbi_state.csv
│   └── powerbi_category.csv
├── charts/
│   ├── 01_monthly_revenue.png
│   ├── 02_state_revenue.png
│   ├── 03_categories.png
│   ├── 04_order_status.png
│   ├── 05_review_delivery.png
│   ├── 06_payment.png
│   └── 07_delivery_dist.png
├── Olist_EDA.ipynb
├── Olist_Dashboard.pbix
└── README.md
```

---

## 🚀 Hướng dẫn chạy

```bash
# 1. Clone repo
git clone https://github.com/<your-username>/olist-eda.git
cd olist-eda

# 2. Tạo môi trường Conda
conda create -n olist-eda python=3.10
conda activate olist-eda

# 3. Cài thư viện
pip install pandas numpy matplotlib seaborn jupyter ipykernel

# 4. Tải dataset từ Kaggle vào thư mục data/
# https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

# 5. Chạy notebook
jupyter notebook Olist_EDA.ipynb
```

---

## 🔍 Quy trình phân tích (EDA)

### Bước 1 — Load & Khám phá dữ liệu

- Load 9 file CSV gốc
- Kiểm tra cấu trúc và missing values từng bảng

### Bước 2 — Làm sạch & Merge

- Chuyển đổi kiểu dữ liệu datetime
- Merge 9 bảng thành 1 dataframe tổng
- Lọc chỉ giữ đơn hàng đã giao thành công
- Tạo các cột phân tích: `year_month`, `delivery_days`, `weekday`

### Bước 3 — Phân tích & Visualization (7 góc phân tích)

1. Doanh thu theo tháng
2. Doanh thu theo bang (địa lý)
3. Top 15 danh mục sản phẩm
4. Phân bổ trạng thái đơn hàng
5. Review score vs thời gian giao hàng
6. Phương thức thanh toán
7. Phân phối thời gian giao hàng

### Bước 4 — Export & Dashboard

- Export 4 file CSV sạch cho Power BI
- Xây dựng Dashboard 3 trang trong Power BI

---

## 💡 Insights chính

### 📈 Doanh thu

- Tổng doanh thu đạt **~19.87M BRL** trong 2 năm
- Doanh thu tăng trưởng mạnh từ **Q1/2017**, đạt đỉnh vào **Q4/2017**
- Trung bình mỗi đơn hàng đạt **206 BRL**

### 🗺️ Địa lý

- **São Paulo (SP)** chiếm ~40% tổng doanh thu toàn quốc
- Top 3 bang: **SP → RJ → MG** — tập trung ở vùng Đông Nam Brazil
- Các bang vùng Bắc (RR, AP, AM) có thời gian giao hàng dài nhất (~25–30 ngày)

### 🛍️ Sản phẩm

- **bed_bath_table** là danh mục dẫn đầu về doanh thu
- **health_beauty** và **computers_accessories** nằm trong top 3
- Có sự chênh lệch giữa doanh thu và số lượng đơn theo từng danh mục

### 🚚 Giao hàng & Hài lòng

- Thời gian giao hàng trung bình: **~18 ngày**
- Bang giao hàng **càng nhanh → review score càng cao**
- Đơn hàng giao dưới 5 ngày có **70% khách cho 5 sao**
- Đơn hàng giao trên 20 ngày có review score trung bình chỉ **~2.8/5**

### 💳 Thanh toán

- **Credit card** chiếm ~74% tổng giao dịch
- **Boleto** (thanh toán tiền mặt) đứng thứ 2 với ~19%

---

## 📊 Power BI Dashboard

Dashboard gồm 3 trang:

| Trang | Nội dung |
|---|---|
| Overview | 5 KPI Cards + Line Chart doanh thu + Bar Chart Top 10 States |
| Product Analysis | Bar Chart Top 15 Categories + Table + Donut Chart |
| Delivery & Satisfaction | Bar Chart Delivery by State + Column Chart Review + Scatter Chart |

---

## 👤 Tác giả

**Trần Tấn Phát**

- 📧 trantanphat08112004@gmail.com
- 🎓 Sinh viên Công nghệ Dữ liệu — Đại học Văn Lang
- 💼 GitHub: [github.com/trantanphat0811](https://github.com/trantanphat0811)

---

## 📄 License

Dataset thuộc sở hữu của [Olist](https://olist.com/) và được công bố công khai trên Kaggle theo giấy phép **CC BY-NC-SA 4.0**.
