FROM python:3.11-slim-bookworm

RUN apt-get update && apt-get upgrade -y && apt-get install --no-install-recommends -y \
	ca-certificates \
	&& apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY app.py .

RUN adduser --system --group --uid 1001 appuser
USER appuser

CMD ["python", "app.py"]