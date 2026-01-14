# ---------- Stage 1: Builder ----------
FROM python:3.9-slim AS builder

WORKDIR /app

RUN apt-get update && apt-get install -y \
    gcc \
    default-libmysqlclient-dev \
    pkg-config \
 && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip install --upgrade pip && \
    pip wheel --no-cache-dir -r requirements.txt


# ---------- Stage 2: Runtime ----------
FROM python:3.9-slim

WORKDIR /app

# 🔑 REQUIRED for mysqlclient
RUN apt-get update && apt-get install -y \
    libmariadb3 \
    default-libmysqlclient-dev \
    pkg-config \
 && rm -rf /var/lib/apt/lists/*

COPY --from=builder /app /app

RUN pip install --no-cache-dir *.whl

COPY . .

EXPOSE 5000
CMD ["python", "app.py"]
