FROM python:3.13-slim AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

FROM python:3.13-slim

WORKDIR /app

ENV PYTHONUNBUFFERED=1

LABEL org.opencontainers.image.source="https://github.com/pablinrlq/linuxtips-workspace" \
      org.opencontainers.image.title="giropops-status" \
      org.opencontainers.image.description="Dashboard de status de servicos (LINUXtips)" \
      org.opencontainers.image.version="1.0.0" \
      org.opencontainers.image.licenses="MIT"

RUN groupadd --system --gid 1000 giropops \
 && useradd --system --uid 1000 --gid giropops --no-create-home --shell /usr/sbin/nologin giropops

COPY --from=builder /install /usr/local

COPY . .

EXPOSE 5000

HEALTHCHECK --interval=10s --timeout=3s --start-period=5s --retries=3 \
  CMD python -c "import urllib.request,sys; sys.exit(0 if urllib.request.urlopen('http://localhost:5000/health', timeout=2).status == 200 else 1)"

USER giropops

CMD ["python", "app.py"]
