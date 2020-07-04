FROM mysql:8.0

ENV MYSQL_ROOT_PASSWORD=secret

RUN echo "America/Sao_Paulo" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata

WORKDIR /var/www/html