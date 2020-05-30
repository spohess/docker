FROM python:3.8

LABEL maintainer="Sergio Hess <hess.sergio@gmail.com>"

ENV PYTHONUNBUFFERED 1

RUN useradd -s /bin/bash -u 1000 spohess && \
    mkdir /code && \
    chown -R spohess:spohess /code && \
    echo "spohess  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers

COPY requirements.txt .

RUN pip install -r requirements.txt

USER spohess

WORKDIR "/code"