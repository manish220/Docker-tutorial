From ubuntu:latest

WORKDIR /app

COPY app.py /app/app.py

RUN apt-get update && apt-get install -y python3 python3-pip

ENV NAME World

CMD ["python3", "/app/app.py"]