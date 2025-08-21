# ---------- Build stage ----------
FROM python:3.10-slim AS builder

# Set the working directory inside the container to /app
WORKDIR /app

# Copy app.py into the working directory of this stage
COPY app.py .

# Install psycopg2-binary into a custom folder (/tmp/libs) instead of globally
# This way, only the required library files are collected and can be copied later
RUN pip install --target=/tmp/libs psycopg2-binary


# ---------- Final (runtime) stage ----------
FROM python:3.10-slim

# Set the working directory inside the container to /app
WORKDIR /app

# Copy the application file from the builder stage into this final image
COPY --from=builder /app/app.py .

# Copy the installed dependencies from the builder stage into the Python site-packages directory
# This makes them available to Python in the final runtime container
COPY --from=builder /tmp/libs /usr/local/lib/python3.10/site-packages/

# Run the application
CMD ["python", "app.py"]