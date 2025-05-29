FROM python:3.9-slim-buster

ARG UID=1000
ARG GID=1000

RUN groupadd -g $GID user && useradd -m -u $UID -g user user

WORKDIR /app

RUN echo "import time" > /app/script.py && \
    echo "while True:" >> /app/script.py && \
    echo "    print('Script running as user: $(id -un)')" >> /app/script.py && \
    echo "    time.sleep(5000)" >> /app/script.py && \
    chown user:user /app/script.py

USER user

CMD ["python", "/app/script.py"]