FROM apache/superset:2.0.1

LABEL maintainer="Yuriy Gavrilin <yuriy@kazanexpress.ru>"

USER root
RUN apt-get update && \
    apt-get install --no-install-recommends -y firefox-esr=91.13.0esr-1~deb11u1 libsnappy-dev=1.1.8-1 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir --parents /usr/local/share/ca-certificates/Yandex && \
    wget -q "https://storage.yandexcloud.net/cloud-certs/CA.pem" --output-document /usr/local/share/ca-certificates/Yandex/YandexInternalRootCA.crt && \
    chmod 655 /usr/local/share/ca-certificates/Yandex/YandexInternalRootCA.crt

ENV GECKODRIVER_VERSION=0.29.0
RUN wget -q https://github.com/mozilla/geckodriver/releases/download/v${GECKODRIVER_VERSION}/geckodriver-v${GECKODRIVER_VERSION}-linux64.tar.gz && \
    tar -x geckodriver -zf geckodriver-v${GECKODRIVER_VERSION}-linux64.tar.gz -O > /usr/bin/geckodriver && \
    chmod 755 /usr/bin/geckodriver && \
    rm geckodriver-v${GECKODRIVER_VERSION}-linux64.tar.gz

COPY requirements.txt /additional_requirements.txt
RUN pip install --no-cache --no-cache-dir -r /additional_requirements.txt && \
    pip install --no-cache --no-cache-dir prophet==1.0.1 && \
    pip install --no-cache --no-cache-dir --force-reinstall git+https://github.com/benoitc/gunicorn.git@1efc5d91fa3dd2fad54cb931a9f7473499de5165

USER superset