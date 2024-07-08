FROM ubuntu:latest

# Set working directory
WORKDIR /app

RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    curl \
    file \
    git \
    python3 \
    python3-pip \
    python3-venv \
    postgresql \
    postgresql-contrib && \
    apt-get clean

# Copy application files
COPY . .

# Verify that requirements.txt exists
RUN if [ ! -f requirements.txt ]; then echo "requirements.txt not found"; exit 1; fi

# Create and activate a virtual environment
RUN python3 -m venv /app/venv
ENV PATH="/app/venv/bin:$PATH"

# Install Python dependencies within the virtual environment
RUN pip install --upgrade pip setuptools wheel
RUN pip install --no-cache-dir -r requirements.txt

# Set environment variables
ENV DB_USERNAME=myuser
ENV DB_PASSWORD=mypassword
ENV DB_HOST=host.docker.internal
ENV DB_PORT=5433
ENV DB_NAME=mydatabase

EXPOSE 5153

# Command to run the application
CMD ["python3", "app.py"]
