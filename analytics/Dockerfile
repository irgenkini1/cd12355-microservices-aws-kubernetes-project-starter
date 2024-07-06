FROM python:3.10-slim-buster

WORKDIR /app

COPY . /app

RUN pip3 install --no-cache-dir -r requirements.txt

CMD ["python", "app.py"]