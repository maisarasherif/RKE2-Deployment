# Use Python 3.11 slim image as base
FROM python:3.11-slim

# Build arguments (passed from CI/CD)
ARG BUILD_DATE="unknown"
ARG VCS_REF="unknown" 
ARG VERSION="unknown"

# Set build info as environment variables
ENV BUILD_DATE=$BUILD_DATE \
    VCS_REF=$VCS_REF \
    VERSION=$VERSION \
    PYTHONUNBUFFERED=1 \
    PORT=5000

# Add labels for metadata
LABEL org.opencontainers.image.created=$BUILD_DATE \
      org.opencontainers.image.source="https://github.com/maisarasherif/RKE2-Deployment.git" \
      org.opencontainers.image.version=$VERSION \
      org.opencontainers.image.revision=$VCS_REF \
      org.opencontainers.image.title="Flask CI/CD Demo" \
      org.opencontainers.image.description="Flask app with CI/CD pipeline"

# Set working directory
WORKDIR /app

# Create non-root user for security
RUN groupadd -r appuser && useradd -r -g appuser appuser

# Install curl for health checks
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements first for better layer caching
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app.py .

# Change ownership to non-root user
RUN chown -R appuser:appuser /app

# Switch to non-root user
USER appuser

# Expose port 5000
EXPOSE 5000

# Add health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:5000/health || exit 1

# Run the application
CMD ["python", "app.py"]
