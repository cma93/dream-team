FROM python:3-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/


# Set the working directory in the container
WORKDIR /code

# COPY requirements.txt .
# RUN pip install --no-cache-dir --upgrade -r requirements.txt


# Enable bytecode compilation
ENV UV_COMPILE_BYTECODE=1

# Copy from the cache instead of linking since it's a mounted volume
ENV UV_LINK_MODE=copy


# RUN playwright install --with-deps chromium
# Copy the rest of the application code into the container
# COPY . .

COPY . /code
RUN uv lock && uv sync --no-dev

# Place executables in the environment at the front of the path
ENV PATH="/code/.venv/bin:$PATH"

# Install Playwright runtime libs with current package names, then browsers
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates && rm -rf /var/lib/apt/lists/*

RUN python -m pip install --upgrade pip playwright \
 && playwright install-deps chromium \
 && playwright install chromium

# Expose the port that the app will run on
EXPOSE 3100

# Command to run the application using Uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "3100", "--workers", "4"]