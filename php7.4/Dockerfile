FROM ubuntu:20.04

# Repository/Image Maintainer
LABEL maintainer="Sergio Hess <hess.sergio@gmail.com>"

# Variables
ENV FRAMEWORK=laravel \
    DEBIAN_FRONTEND=noninteractive \
    OPCACHE_MODE="normal" \
    XDEBUG_ENABLED=false \
    TERM=xterm-256color \
    COLORTERM=truecolor \
    COMPOSER_PROCESS_TIMEOUT=1200 \
    TZ=America/Sao_Paulo

# Add the ENTRYPOINT script
ADD start.sh /scripts/start.sh
ADD bashrc /home/spohess/.bashrc
ADD exa /usr/local/bin

RUN echo "---> Instalando Dependências Básicas" \
    && apt-get update && apt-get install -y --no-install-recommends apt-utils tzdata \
    && ln -fs /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime \
    && dpkg-reconfigure tzdata \
    && date

# Install PHP From DotDeb, Common Extensions, Composer and then cleanup
RUN echo "---> Instalando PHP" \
    && apt-get install -y \
    software-properties-common \
    gcc \
    g++ \
    make \
    autoconf \
    libc-dev \
    pkg-config \
    unixodbc-dev \
    curl \
    bash \
    fontconfig \
    imagemagick \
    nano \
    vim \
    git \
    unzip \
    wget \
    sudo \
    && echo "---> DESABILITANDO A INTERATIVIDADE COM O TERMINAL" \
    && apt-get install -y \
    php7.4 \
    php7.4-dev \
    php7.4-apcu \
    php7.4-bcmath \
    php7.4-bz2 \
    php7.4-calendar \
    php7.4-cli \
    php7.4-common \
    php7.4-ctype \
    php7.4-curl \
    php7.4-exif \
    php7.4-fpm \
    php7.4-gd \
    php7.4-gmp \
    php7.4-iconv \
    php7.4-imagick \
    php7.4-imap \
    php7.4-intl \
    php7.4-json \
    php7.4-mailparse \
    php7.4-mbstring \
    php7.4-mysqlnd \
    php7.4-mysql \
    php7.4-mongodb \
    php7.4-opcache \
    php7.4-pgsql \
    php7.4-posix \
    php7.4-readline \
    php7.4-redis \
    php7.4-soap \
    php7.4-sqlite3 \
    php7.4-xdebug \
    php7.4-xml \
    php7.4-xmlreader \
    php7.4-phar \
    php7.4-zip \
    php7.4-phpdbg \
    php-pear \
    openssl \
    && echo "---> Instalando o Composer" \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && echo "---> Adicionado o usuário spohess" \
    && useradd -s /bin/bash -u 1000 spohess \
    && mkdir -p /var/www/html \
    && chown -R spohess:spohess /home/spohess \
    && chown -R spohess:spohess /var/www \
    && echo "---> Configurando o PHP" \
    && echo "spohess  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers \
    && sed -i "/user = .*/c\;user = spohess" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/^group = .*/c\;group = spohess" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/listen.owner = .*/c\;listen.owner = spohess" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/listen.group = .*/c\;listen.group = spohess" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/listen = .*/c\listen = [::]:9000" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/;access.log = .*/c\access.log = /proc/self/fd/2" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/;clear_env = .*/c\clear_env = no" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/;catch_workers_output = .*/c\catch_workers_output = yes" /etc/php/7.4/fpm/pool.d/www.conf \
    && sed -i "/pid = .*/c\;pid = /run/php/php/7.4/fpm.pid" /etc/php/7.4/fpm/php-fpm.conf \
    && sed -i "/;daemonize = .*/c\daemonize = yes" /etc/php/7.4/fpm/php-fpm.conf \
    && sed -i "/error_log = .*/c\error_log = /proc/self/fd/2" /etc/php/7.4/fpm/php-fpm.conf \
    && sed -i "/post_max_size = .*/c\post_max_size = 1000M" /etc/php/7.4/cli/php.ini \
    && sed -i "/upload_max_filesize = .*/c\upload_max_filesize = 1000M" /etc/php/7.4/cli/php.ini \
    && sed -i "/zend_extension=xdebug.so/c\;zend_extension=xdebug.so" /etc/php/7.4/cli/conf.d/20-xdebug.ini \
    && chmod +x /scripts/start.sh \
    && rm -rf /tmp/*

RUN echo "---> Instalando MSSQL" \
    && curl -s https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && bash -c "curl -s https://packages.microsoft.com/config/ubuntu/19.10/prod.list > /etc/apt/sources.list.d/mssql-release.list" \
    && apt-get update \
    && ACCEPT_EULA=Y apt-get -y install mssql-tools unixodbc-dev \
    && pecl install sqlsrv \
    && pecl install pdo_sqlsrv \
    && printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/7.4/mods-available/sqlsrv.ini \
    && printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/7.4/mods-available/pdo_sqlsrv.ini \
    && phpenmod -v 7.4 sqlsrv pdo_sqlsrv

# Define the running user
USER spohess

RUN composer global require laravel/installer

# Application directory
WORKDIR "/var/www/html"

# Environment variables
ENV PATH=/home/spohess/.composer/vendor/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Define um entry point
ENTRYPOINT ["/scripts/start.sh"]

CMD ["/bin/bash"]