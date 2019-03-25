FROM ubuntu:18.04

# Repository/Image Maintainer
LABEL maintainer="Sergio Hess <hess.sergio@gmail.com>"

# Variables
ENV FRAMEWORK=laravel \
    OPCACHE_MODE="normal" \
    XDEBUG_ENABLED=false \
    TERM=xterm-256color \
    COLORTERM=truecolor \
    COMPOSER_PROCESS_TIMEOUT=1200

# Add the ENTRYPOINT script
ADD start.sh /scripts/start.sh
ADD bashrc /home/spohess/.bashrc

# Install PHP From DotDeb, Common Extensions, Composer and then cleanup
RUN echo "---> Instalando Dependências Básicas" && \
    apt-get update -y && \
    apt-get install -y \
    software-properties-common \
    curl \
    bash \
    fontconfig \
    imagemagick \
    nano \
    vim \
    git \
    unzip \
    wget \
    make \
    sudo && \
    echo "---> DESABILITANDO A INTERATIVIDADE COM O TERMINAL" && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt-get update -y && \
    apt-get install -y \
    php7.2 \
    php7.2-apcu \
    php7.2-bcmath \
    php7.2-bz2 \
    php7.2-calendar \
    php7.2-cli \
    php7.2-common \
    php7.2-ctype \
    php7.2-curl \
    php7.2-exif \
    php7.2-fpm \
    php7.2-gd \
    php7.2-gmp \
    php7.2-iconv \
    php7.2-imagick \
    php7.2-imap \
    php7.2-intl \
    php7.2-json \
    php7.2-mailparse \
    php7.2-mbstring \
    php7.2-mysqlnd \
    php7.2-mysql \
    php7.2-mongodb \
    php7.2-opcache \
    php7.2-pgsql \
    php7.2-posix \
    php7.2-readline \
    php7.2-redis \
    php7.2-soap \
    php7.2-sqlite3 \
    php7.2-xdebug \
    php7.2-xml \
    php7.2-xmlreader \
    php7.2-phar \
    php7.2-zip \
    php7.2-phpdbg && \
    openssl && \
    echo "---> Instalando o Composer" && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    echo "---> Adicionado o usuário spohess" && \
    useradd -s /bin/bash -u 1000 spohess && \
    mkdir -p /var/www/html && \
    chown -R spohess:spohess /home/spohess && \
    chown -R spohess:spohess /var/www && \
    echo "---> Configurando o PHP" && \
    echo "spohess  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers && \
    sed -i "/user = .*/c\;user = spohess" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/^group = .*/c\;group = spohess" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/listen.owner = .*/c\;listen.owner = spohess" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/listen.group = .*/c\;listen.group = spohess" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/listen = .*/c\listen = [::]:9000" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/;access.log = .*/c\access.log = /proc/self/fd/2" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/;clear_env = .*/c\clear_env = no" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/;catch_workers_output = .*/c\catch_workers_output = yes" /etc/php/7.2/fpm/pool.d/www.conf && \
    sed -i "/pid = .*/c\;pid = /run/php/php/7.2/fpm.pid" /etc/php/7.2/fpm/php-fpm.conf && \
    sed -i "/;daemonize = .*/c\daemonize = yes" /etc/php/7.2/fpm/php-fpm.conf && \
    sed -i "/error_log = .*/c\error_log = /proc/self/fd/2" /etc/php/7.2/fpm/php-fpm.conf && \
    sed -i "/post_max_size = .*/c\post_max_size = 1000M" /etc/php/7.2/cli/php.ini && \
    sed -i "/upload_max_filesize = .*/c\upload_max_filesize = 1000M" /etc/php/7.2/cli/php.ini && \
    sed -i "/zend_extension=xdebug.so/c\;zend_extension=xdebug.so" /etc/php/7.2/cli/conf.d/20-xdebug.ini && \
    chmod +x /scripts/start.sh && \
    rm -rf /tmp/*

# Define the running user
USER spohess

# Application directory
WORKDIR "/var/www/html"

# Environment variables
ENV PATH=/home/spohess/.composer/vendor/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Define um entry point
ENTRYPOINT ["/scripts/start.sh"]

CMD ["/bin/bash"]