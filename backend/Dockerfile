# 1) Stage with uv binaries
FROM ghcr.io/astral-sh/uv:latest AS uvbin

# 2) Final app image with browsers & deps preinstalled
FROM mcr.microsoft.com/playwright/python:v1.52.0-jammy

# add uv
COPY --from=uvbin /uv /uvx /bin/

WORKDIR /code
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy UV_PYTHON=3.12

# better build caching: lock first, then app
COPY pyproject.toml uv.lock ./
RUN --mount=type=cache,target=/root/.cache/uv uv sync --locked --no-dev
COPY . .
ENV PATH="/code/.venv/bin:$PATH"

EXPOSE 3100
CMD ["uvicorn","main:app","--host","0.0.0.0","--port","3100","--workers","4"]
