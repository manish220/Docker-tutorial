
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

COPY calculator.py .
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

CMD ["python", "calculator.py"]

Why we use it:

Simple and easy to understand.

Good for development and learning.

Includes everything needed to build and run the app.

❌ Cons:
Slightly larger image

Includes build-time tools you don’t need in production

'''🐳 Multi-Stage Dockerfile'''
dockerfile

# Stage 1: Build
FROM python:3.10-slim as builder

WORKDIR /app
COPY calculator.py .
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt \
 && mkdir -p /out && cp -r . /out

# Stage 2: Final
FROM python:3.10-slim

WORKDIR /app
COPY --from=builder /out /app

CMD ["python", "calculator.py"]

Why multistage?
Reduces image size.

Separates build from production.

Safer and cleaner for deployment.


❌ Cons:
Slightly more complex to write

Overhead for very small/simple apps

🧊 What Is Distroless?
Distroless images include only your app and its runtime. No shell, no package manager, no bloat.

📦 Example:
dockerfile

FROM gcr.io/distroless/python3
COPY calculator.py .
CMD ["calculator.py"]

✅ Benefits:
Minimal size
Fewer vulnerabilities
Faster startup

❌ Downsides:
No shell inside container (harder to debug)
No interactive CLI access

🔍 Docker Image Strategy Comparison
| Feature    | Regular Dockerfile | Multistage Dockerfile | Distroless        |
| ---------- | ------------------ | --------------------- | ----------------- |
| Simplicity | ✅ Easy             | 🔶 Medium             | ❌ Advanced        |
| Image Size | 🔶 Medium (\~50MB) | ✅ Smaller             | ✅ Smallest        |
| Debugging  | ✅ Yes              | 🔶 Moderate           | ❌ No shell        |
| Best For   | Learning, Dev      | Production            | Secure Production |

🚀 How to Build and Run
With Regular Dockerfile:

docker build -t calc:basic -f Dockerfile .
docker run -it calc:basic

With Multistage Dockerfile:

docker build -t calc:multi -f Dockerfile.multi .
docker run -it calc:multi
