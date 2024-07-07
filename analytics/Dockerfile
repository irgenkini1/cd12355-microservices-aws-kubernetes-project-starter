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
ENV DB_HOST=127.0.0.1
ENV DB_PORT=5433
ENV DB_NAME=mydatabase

# Initialize and seed the database
RUN service postgresql start && \
    su - postgres -c "psql -c \"CREATE USER ${DB_USERNAME} WITH PASSWORD '${DB_PASSWORD}';\"" && \
    su - postgres -c "psql -c \"CREATE DATABASE ${DB_NAME};\"" && \
    su - postgres -c "psql -c \"GRANT ALL PRIVILEGES ON DATABASE ${DB_NAME} TO ${DB_USERNAME};\""

# Copy SQL files into the container
COPY 1_create_tables.sql /app
COPY 2_seed_users.sql /app
COPY 3_seed_tokens.sql /app

# Run SQL scripts
RUN PGPASSWORD="${DB_PASSWORD}" psql --host "${DB_HOST}" -p "${DB_PORT}" -U "${DB_USERNAME}" -d "${DB_NAME}" < /app/1_create_tables.sql && \
    PGPASSWORD="${DB_PASSWORD}" psql --host "${DB_HOST}" -p "${DB_PORT}" -U "${DB_USERNAME}" -d "${DB_NAME}" < /app/2_seed_users.sql && \
    PGPASSWORD="${DB_PASSWORD}" psql --host "${DB_HOST}" -p "${DB_PORT}" -U "${DB_USERNAME}" -d "${DB_NAME}" < /app/3_seed_tokens.sql

# Command to run the application
CMD ["python3", "app.py"]
