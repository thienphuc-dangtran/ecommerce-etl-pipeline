# Đồ án nhỏ: Luồng xử lý dữ liệu E-Commerce (ETL Pipeline)

## Giới thiệu dự án
Đây là một project nhỏ mình tự xây dựng để thực hành kỹ năng Data Engineering. Thay vì chỉ làm việc với một file CSV đã được dọn sẵn như trong các bài toán phân tích thông thường, mình muốn thử sức với kịch bản dữ liệu thực tế hơn: thông tin khách hàng và sản phẩm bị phân tán ở nhiều bảng khác nhau.

Vì laptop cá nhân đang dùng chỉ có 8GB RAM, để tránh việc máy bị đơ khi xử lý hàng trăm ngàn dòng dữ liệu, mình đã thiết lập toàn bộ quy trình tự động này chạy trực tiếp trên máy chủ của Google Colab.

## Công cụ sử dụng
* **Ngôn ngữ:** Python (thư viện Pandas, SQLite3)
* **Nguồn dữ liệu:** API của Kaggle (Bộ dữ liệu Brazilian E-Commerce của Olist)
* **Môi trường chạy:** Google Colab

## Quá trình xây dựng luồng dữ liệu

**1. Thu thập (Extract):**
Thay vì tải thủ công cục dữ liệu lớn về máy, mình đã viết script dùng Kaggle API để tự động kéo 3 bảng dữ liệu cốt lõi (`Orders`, `Order Items` và `Products`) thẳng vào môi trường Colab.

**2. Làm sạch & Nối bảng (Transform):**
* Lọc bỏ các đơn hàng bị hủy và xóa các dòng bị khuyết thiếu ngày giao.
* Sử dụng Pandas thực hiện lệnh `INNER JOIN` để gộp 3 bảng rời rạc lại thành một luồng thống nhất thông qua khóa `order_id` và `product_id`.
* Lược bỏ bớt các cột dư thừa để bảng dữ liệu gọn gàng và tối ưu bộ nhớ nhất.

**3. Lưu trữ (Load):**
Mình tự khởi tạo một cơ sở dữ liệu ảo SQLite (`ecommerce_cleaned.db`) ngay trên Colab và đổ toàn bộ dữ liệu đã làm sạch (hơn 110.000 dòng) vào đó. Dữ liệu này giờ đã sẵn sàng cho việc truy vấn phân tích hoặc làm đầu vào cho các thuật toán gợi ý.

## Test nhanh hệ thống bằng SQL
Để kiểm chứng xem luồng dữ liệu chạy có mượt không, mình đã viết một câu truy vấn SQL tìm ra Top 5 ngành hàng có doanh thu cao nhất. Kết quả in ra hoàn toàn chuẩn xác:

<img width="410" height="262" alt="image" src="https://github.com/user-attachments/assets/ee02ceb7-d05e-43d0-b54b-92a34ead8e71" />
