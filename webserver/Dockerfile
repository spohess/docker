FROM spohess/php7.4:1.0.0

# Repository/Image Maintainer
LABEL maintainer="Sergio Hess <hess.sergio@gmail.com>"

# Reset usuário para o root, para poder install os pacotes
USER root

ENV APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2 \
    APACHE_PID_FILE=/var/run/apache2.pid \
    APACHE_RUN_DIR=/var/run/apache2 \
    APACHE_LOCK_DIR=/var/lock/apache2 \
    APACHE_DOCUMENT_ROOT=/var/www/html/public

RUN echo "Atualizando pacotes e instalando Apache" && \
    apt-get update -y && \
    apt-get install -y \
    apache2 \
    libapache2-mod-fcgid && \
    echo "HABILITANDO O REWRITE E O FASTCGI" && \
    a2enmod rewrite && \
    a2enmod actions fcgid alias proxy_fcgi && \
    echo "Limpando pastas Desnecessáris" && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* && \
    echo "Liberando permissões" && \
    mkdir /var/run/apache2 && \
    mkdir /var/lock/apache2 && \
    chown -R spohess:www-data /var/run/apache2 && \
    chown -R spohess:www-data /var/log/apache2 && \
    chown -R spohess:www-data /var/lock/apache2 && \
    chown -R spohess:www-data /var/lib/apache2 && \
    chown -R spohess:www-data /var/www && \
    chmod 775 -R /var

COPY conf/000-default.conf /etc/apache2/sites-available/000-default.conf
COPY conf/ports.conf /etc/apache2/ports.conf
COPY conf/apache2.conf /etc/apache2/apache2.conf
COPY start.sh  /home/spohess/start.sh

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf && \
    sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf && \
    sed -i 's/Listen\ 80/Listen\ 8080/g' /etc/apache2/ports.conf && \
    sed -i 's/\*\:80/\*\:8080/g' /etc/apache2/sites-available/*.conf

RUN chmod +x /home/spohess/start.sh && chown -R spohess:spohess /home/spohess

# Application directory
WORKDIR "/var/www/html"

# Define the running user
USER spohess

CMD ["/home/spohess/start.sh"]