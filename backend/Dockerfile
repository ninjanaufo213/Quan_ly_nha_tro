# Stage 1 Build stage
FROM python:3.13-slim AS builder

WORKDIR /app


RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements và cài đặt vào một thư mục tạm
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=install -r requirements.txt
# Cài thêm các thư viện còn thiếu bạn vừa gặp
RUN pip install --no-cache-dir --prefix=install email-validator bcrypt==4.0.1 pymysql

# Stage 2 Production stage
FROM python:3.13-slim

WORKDIR /app

# Copy các thư viện đã build từ stage 1
COPY --from=builder /app/install /usr/local

# Copy toàn bộ mã nguồn vào
COPY . .
# Thiết lập biến môi trường để tránh tạo file .pyc
ENV PYTHONDONTWRITEBYTECODE=1

# Mở cổng 8000
EXPOSE 8000

# Lệnh chạy ứng dụng (sử dụng uvicorn trực tiếp)
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]