
FROM python:3.13-slim AS builder

WORKDIR /app


RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    gcc \
    && rm -rf /var/lib/apt/lists/*


COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=install -r requirements.txt

RUN pip install --no-cache-dir --prefix=install email-validator bcrypt==4.0.1 pymysql


FROM python:3.13-slim

WORKDIR /app


COPY --from=builder /app/install /usr/local


COPY . .

ENV PYTHONDONTWRITEBYTECODE=1


EXPOSE 8000


CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]