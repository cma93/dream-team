FROM mcr.microsoft.com/playwright/python:v1.52.0-jammy
WORKDIR /code
COPY pyproject.toml uv.lock ./
RUN uv sync --locked --no-dev
COPY . .
ENV PATH="/code/.venv/bin:$PATH"
EXPOSE 3100
CMD ["uvicorn","main:app","--host","0.0.0.0","--port","3100","--workers","4"]
