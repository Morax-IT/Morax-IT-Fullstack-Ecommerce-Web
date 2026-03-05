# Fullstack E-commerce Web Project

## 1. Tổng quan dự án (Project Overview)
Đây là một dự án ứng dụng thương mại điện tử Fullstack, cung cấp các tính năng cốt lõi của một trang mua sắm trực tuyến như duyệt sản phẩm, phân loại danh mục, tìm kiếm, giỏ hàng, và quy trình thanh toán (checkout).

Dự án được xây dựng với kiến trúc Client-Server rõ ràng, đảm bảo tính mở rộng và dễ dàng bảo trì. Nó tích hợp xác thực bảo mật chuẩn OAuth2 và được container hóa hoàn toàn để triển khai dễ dàng trên mọi môi trường.

---

## 2. Công nghệ sử dụng (Technology Stack)

Dự án này ứng dụng các công nghệ hiện đại ở cả Frontend, Backend và quy trình vận hành:

### Frontend (User Interface)
*   **Framework:** Angular (phiên bản 16+)
*   **Ngôn ngữ:** TypeScript, HTML5, CSS3
*   **Thư viện UI/UX:** Bootstrap CSS
*   **Xác thực:** `@okta/okta-angular` & `@okta/okta-signin-widget`

### Backend (REST API)
*   **Framework:** Spring Boot (Java 17)
*   **Data Access:** Spring Data JPA, Spring Data REST (tự động hóa việc tạo API từ Entity)
*   **Database Integration:** MySQL Driver (JDBC)
*   **Security:** Spring Security & Okta OAuth2 Resource Server
*   **Build Tool:** Maven

### Cơ sở dữ liệu (Database)
*   **DBMS:** MySQL 8.0
*   Cấu trúc dữ liệu bao gồm các bảng: Sản phẩm, Danh mục, Đơn hàng, Địa chỉ giao hàng (Quốc gia/Tiểu bang). Dữ liệu mẫu cung cấp sẵn qua scripts migration tự động chạy khi khởi tạo Docker.

### Hệ thống & Vận hành (Infrastructure/DevOps)
*   **Containerization:** Docker (sử dụng Dockerfile multi-stage build cho cả Frontend và Backend).
*   **Orchestration:** Docker Compose (Quản lý cụm container MySQL, Spring Boot, Angular, và Nginx).
*   **Reverse Proxy / Web Server:** Nginx (Đóng vai trò điều phối traffic giữa tên miền Frontend và Backend API, giải quyết vấn đề CORS).

---

## 3. Hướng dẫn triển khai trên môi trường Local (Docker Desktop)

Nhờ có Docker Compose, bạn có thể triển khai toàn bộ hệ thống (Database + Backend + Frontend + Proxy) trên máy tính cá nhân chỉ với vài dòng lệnh mà không cần cài đặt Node.js hay Java JDK trên máy chủ (host).

### Yêu cầu tiên quyết (Prerequisites)
1. Máy tính của bạn cần cài sẵn **[Docker Desktop](https://www.docker.com/products/docker-desktop/)**.
2. Đã bật Docker (biểu tượng cá voi màu xanh/chạy ngầm dưới khay hệ thống).

### Bước 1: Ánh xạ tên miền bằng file `hosts`
Do dự án này sử dụng Nginx Proxy và cấu hình cứng tên miền, bạn cần sửa file hosts trên máy tính để máy hiểu các tên miền ảo này trỏ về chính máy tính của bạn (Localhost - `127.0.0.1`).

*   **Trên Windows:**
    Mở Notepad **bằng quyền Administrator** (Run as Administrator), sau đó mở file theo đường dẫn: `C:\Windows\System32\drivers\etc\hosts`
*   **Trên MacOS / Linux:**
    Mở Terminal và gõ: `sudo nano /etc/hosts`

Thêm 1 dòng sau vào cuối file và lưu lại:
```text
127.0.0.1 api-ecommerce.duchmedu.vn ecommerce.duchmedu.vn
```

### Bước 2: Khởi chạy dự án bằng Docker Compose
1. Mở Terminal / Command Prompt.
2. Điều hướng vào thư mục gốc của dự án (nơi chứa file `docker-compose.yml`).
3. Chạy lệnh sau để build image và khởi động các container:
   ```bash
   docker-compose up -d --build
   ```
   *(Quá trình build lần đầu có thể mất vài phút để tải các thư viện Maven và NPM. Cờ `-d` giúp chạy nền ẩn).*

### Bước 3: Truy cập và sử dụng
1. Đợi khoảng 1 - 2 phút để cơ sở dữ liệu MySQL insert xong dữ liệu mẫu và các server khởi động hoàn thành.
2. Mở trình duyệt web của bạn và truy cập:
   👉 **[http://ecommerce.duchmedu.vn](http://ecommerce.duchmedu.vn)**
   *(Nếu bạn click vào "Login", hệ thống sẽ chuyển hướng sang trang đăng nhập bảo mật của Okta)*.

---

### Phụ lục: Các lệnh Docker hữu ích

Nếu bạn cần kiểm tra lỗi hoặc dừng dự án, dưới đây là một số lệnh thường dùng (chạy trong thư mục chứa file `docker-compose.yml`):

*   **Xem trạng thái các container:**
    ```bash
    docker-compose ps
    ```
*   **Xem log lỗi của Backend (hoặc thay bằng tên dịch vụ khác như `frontend`, `db`, `proxy`):**
    ```bash
    docker-compose logs -f backend
    ```
*   **Dừng (Stop) hệ thống mà không xóa dữ liệu:**
    ```bash
    docker-compose stop
    ```
*   **Tắt (Down) và dọn dẹp hệ thống (Xóa Containers & Networks):**
    ```bash
    docker-compose down
    ```
*   **Xóa SẠCH SẼ cả Database (để làm lại từ đầu):**
    ```bash
    docker-compose down -v
    ```
