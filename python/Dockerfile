FROM python:3.8

# Repository/Image Maintainer
LABEL maintainer="Sergio Hess <hess.sergio@gmail.com>"

ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt && \
	virtualenv venv
COPY . /code/