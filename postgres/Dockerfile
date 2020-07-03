FROM postgres:12.3

ENV POSTGRES_PASSWORD=secret

RUN echo "America/Sao_Paulo" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata

WORKDIR /var