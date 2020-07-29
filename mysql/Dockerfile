FROM mysql:8.0.20

ENV MYSQL_ROOT_PASSWORD=secret \
    DEBIAN_FRONTEND=noninteractive \
    TZ=America/Sao_Paulo

USER root

RUN apt-get update \
    && apt-get install -y apt-utils

RUN ln -fs /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata

RUN apt-get upgrade -y \
    && apt-get install -y \
    net-tools \
    htop

CMD ["--max_allowed_packet=1073741824"]

WORKDIR /var/lib/mysql