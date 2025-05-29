FROM python:3.9 AS builder
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
    && pip install --upgrade pip && \
    pip install --no-cache-dir "flask>=3.1.1" gunicorn && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    /root/.cache/pip

WORKDIR /app
FROM python:3.9-slim-bookworm AS runtime
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN adduser --system --group appuser
USER appuser
WORKDIR /app
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages
COPY --from=builder /usr/local/bin/ /usr/local/bin/
COPY . .
CMD ["gunicorn", "-b", "0.0.0.0:5000", "app:app"]