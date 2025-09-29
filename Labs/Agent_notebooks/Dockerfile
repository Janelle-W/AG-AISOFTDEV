# Stage 1: Install dependencies
FROM python:3.11-slim AS builder

WORKDIR /app

# Copy only the requirements for better caching
COPY Labs/Agent_notebooks/requirements.txt ./requirements.txt
RUN pip install --upgrade pip && \
    pip install --prefix=/install --no-cache-dir -r requirements.txt

# Stage 2: Build image with dependencies and app code
FROM python:3.11-slim

WORKDIR /app

# Copy dependencies from builder stage
COPY --from=builder /install /usr/local

# Copy ONLY what we need (keeps image smaller). We assume build context is repo root.
# If you run docker build from inside Labs/Agent_notebooks, the relative paths below WON'T resolve.
# Recommended build command (from repo root):
#   docker build -f Labs/Agent_notebooks/Dockerfile -t mythology-agent .

# Copy the mythology agent source file from artifacts (single source of truth)
COPY artifacts/app/mythology_agent.py ./mythology_agent.py

# (Optional) Copy any additional support utilities in this folder (notebooks utils, etc.)
COPY Labs/Agent_notebooks/utils.py ./utils.py

# Expose port for FastAPI
EXPOSE 8000

# Default command: run the mythology agent FastAPI app
CMD ["uvicorn", "mythology_agent:app", "--host", "0.0.0.0", "--port", "8000"]