# Stage 1: Use full Python image to "build" (if needed)
FROM python:3.10-slim as builder
WORKDIR /app
COPY calculator.py .
COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt

# Stage 2: Final minimal image
FROM python:3.10-alpine
WORKDIR /app

# Copy only what's needed from builder
COPY --from=builder /app /app

# Set default command
CMD ["python", "calculator.py"]
