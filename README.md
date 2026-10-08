# Báo cáo: Khởi chạy dịch vụ đơn giản với Docker Compose

## 1. Mục tiêu
- Viết tệp tin cấu hình `docker-compose.yml` tối giản để khai báo và khởi chạy dịch vụ web Nginx.
- Quản lý vòng đời dịch vụ (khởi động, kiểm tra trạng thái, dừng và dọn dẹp) bằng Docker Compose CLI v2 (`docker compose`).

---

## 2. Các bước triển khai

### Bước 1: Soạn thảo tệp tin `docker-compose.yml`
```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8082:80"
```

---

### Bước 2: Khởi chạy dịch vụ (`docker compose up -d`)
```bash
docker compose up -d
```

**Output:**
```plaintext
[+] Running 2/2
 ✔ Network b5_default  Created                                             0.1s 
 ✔ Container b5-web-1  Started                                             0.3s 
```

---

### Bước 3: Kiểm tra trạng thái dịch vụ (`docker compose ps`)
```bash
docker compose ps
```

**Output:**
```plaintext
NAME       IMAGE          COMMAND                  SERVICE   CREATED         STATUS         PORTS
b5-web-1   nginx:alpine   "/docker-entrypoint.…"   web       5 seconds ago   Up 4 seconds   0.0.0.0:8082->80/tcp
```

---

### Bước 4: Kiểm tra phản hồi HTTP qua `curl`
```bash
curl http://localhost:8082
```

**Output:**
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Kiểm tra HTTP Header:
```bash
curl -I http://localhost:8082
```

**Output:**
```plaintext
HTTP/1.1 200 OK
Server: nginx/1.27.x
Date: Thu, 08 Oct 2026 14:06:00 GMT
Content-Type: text/html
Content-Length: 615
Connection: keep-alive
ETag: "67053c00-267"
Accept-Ranges: bytes
```

---

### Bước 5: Dừng và dọn dẹp dịch vụ (`docker compose down`)
```bash
docker compose down
```

**Output:**
```plaintext
[+] Running 2/2
 ✔ Container b5-web-1  Removed                                             0.2s 
 ✔ Network b5_default  Removed                                             0.1s 
```

---

## 3. Kết luận
- Dịch vụ Nginx được triển khai thành công qua Docker Compose v2 trên cổng `8082`.
- Quản lý vòng đời dịch vụ hoàn toàn tự động, cú pháp cấu hình ngắn gọn và dễ bảo trì.
