FROM python:3.8

# Repository/Image Maintainer
LABEL maintainer="Sergio Hess <hess.sergio@gmail.com>"

ENV PYTHONUNBUFFERED 1

RUN mkdir /code && \
	cd code

COPY requirements.txt .

RUN pip install -r requirements.txt && \
	virtualenv venv && \
	mv venv /code

# Application directory
WORKDIR "/code"