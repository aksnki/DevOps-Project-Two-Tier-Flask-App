# ---------- Stage 1: Build dependencies ----------
FROM python:3.9-slim AS builder

WORKDIR /app

# Install build dependencies (only here)
RUN apt-get update && apt-get install -y \
    gcc \
    default-libmysqlclient-dev \
    pkg-config \
 && rm -rf /var/lib/apt/lists/*

# Copy only requirements first (layer caching)
COPY requirements.txt .

# Build wheels instead of installing directly
RUN pip install --upgrade pip && \
    pip wheel --no-cache-dir --no-deps -r requirements.txt


# ---------- Stage 2: Runtime image ----------
FROM python:3.9-slim

WORKDIR /app

# Install only runtime dependency for mysqlclient
RUN apt-get update && apt-get install -y \
    libmysqlclient21 \
 && rm -rf /var/lib/apt/lists/*

# Copy wheels from builder stage
COPY --from=builder /app /app

# Install dependencies from wheels
RUN pip install --no-cache-dir *.whl

# Copy application source
COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
