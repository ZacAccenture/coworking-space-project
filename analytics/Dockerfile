FROM python:3.10-slim

WORKDIR /app

# Install packages required to build and connect to PostgreSQL
RUN apt-get update -y && \
    apt-get install -y build-essential libpq-dev && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip install --no-cache-dir --upgrade pip setuptools wheel && \
    pip install --no-cache-dir -r requirements.txt && \
    pip install --no-cache-dir Werkzeug==2.2.3

COPY . .

EXPOSE 5153

CMD ["python", "app.py"]
