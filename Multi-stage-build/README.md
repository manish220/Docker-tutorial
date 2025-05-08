
This project is a simple command-line calculator written in Python. You input two numbers, select an operation (add, subtract, multiply, divide), and get the result.

This README includes:
- A regular Dockerfile
- A multistage Dockerfile
- Explanation of distroless
- Docker Compose usage
- Comparison between Docker image strategies

---

## 📁 Project Structure

calculator-project/
├── calculator.py
├── requirements.txt
├── Dockerfile
└── README.md

```dockerfile
# Dockerfile
FROM python:3.10-slim

WORKDIR /app

 calculator.py .
 requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

CMD ["python", "calculator.py"]
✅ Pros:
Easy to understand and set up

Great for beginners

All-in-one image with both build and run logic

❌ Cons:
Slightly larger image

Includes build-time tools you don’t need in production

🐳 Multi-Stage Dockerfile
dockerfile

Edit
# Dockerfile.multi

# Stage 1: Build
FROM python:3.10-slim as builder

WORKDIR /app

 calculator.py .
 requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt \
    && mkdir -p /out \
    && cp -r . /out

# Stage 2: Final image
FROM python:3.10-slim

WORKDIR /app

 --from=builder /out /app

CMD ["python", "calculator.py"]
✅ Pros:
Allows separation of build and runtime environments

Keeps final image clean and small

Good for production, better security and maintainability

❌ Cons:
Slightly more complex to write

Overhead for very small/simple apps

🧊 What Is Distroless?
Distroless images include only your app and its runtime. No shell, no package manager, no bloat.

📦 Example:
dockerfile

Edit
FROM gcr.io/distroless/python3
 calculator.py .
CMD ["calculator.py"]
✅ Benefits:
Minimal size

Fewer vulnerabilities

Faster startup

❌ Downsides:
No shell inside container (harder to debug)

No interactive CLI access

🔍 Docker Image Strategy Comparison
Feature	Regular Dockerfile	Multi-Stage Dockerfile	Distroless
Simplicity	✅ Easy	🔶 Medium	❌ Complex to debug
Image Size	🔶 Medium (~50MB)	✅ Smaller	✅ Very Small (~15MB)
Debugging	✅ Easy (has shell)	🔶 Limited	❌ No shell or package mgr
Security	🔶 OK	✅ Better	✅ Best
Best Use Case	Learning, testing	Production-ready	Security-sensitive prod

🚀 How to Build and Run
With Regular Dockerfile:


Edit
docker build -t calc:basic -f Dockerfile .
docker run -it calc:basic
With Multistage Dockerfile:


Edit
docker build -t calc:multi -f Dockerfile.multi .
docker run -it calc:multi
🐳 Using Docker Compose
If you want to manage the app with Docker Compose, here's how.

📄 docker-compose.yml
yaml

Edit
version: '3.8'

services:
  calculator:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: python-calculator
    stdin_open: true
    tty: true
▶️ Run with Compose
docker-compose up --build
💡 Use Multistage in Compose
To use the multistage Dockerfile instead, update this line:


Distroless GitHub