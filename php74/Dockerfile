FROM php:7.4-fpm
LABEL maintainer="chin@oberon.nl"

ENV DEBIAN_FRONTEND=noninteractive \
    TERM=xterm \
    PHP_MAX_EXECUTION_TIME=120 \
    PHP_MEMORY_LIMIT=256M \
    PHP_UPLOAD_LIMIT=8M \
    PHP_OPCACHE_LIMIT=64M \
    PHP_OPCACHE_REVALIDATE=2 \
    PHP_SESSION_SAVE_HANDLER=files \
    PHP_SESSION_SAVE_PATH="" \
    PHP_SMTP_HOST=localhost \
    PHP_MAX_INPUT_VARS=1000

COPY settings/production.ini /usr/local/etc/php/conf.d/00-production.ini
COPY settings/docker-settings.ini /usr/local/etc/php/conf.d/01-docker-settings.ini
COPY settings/php-fpm-www.conf /usr/local/etc/php-fpm.d/zz-www-extended.conf

# PHP Configuration
RUN echo "LANG=\"en_US.UTF-8\"" > /etc/default/locale && \
    echo "de_DE.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "es_ES.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "en_GB.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "nl_NL.UTF-8 UTF-8" >> /etc/locale.gen 

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils supervisor cron nano git locales zip openssh-client

# Install image optimizers
RUN apt-get install -y --no-install-recommends jpegoptim libjpeg-turbo-progs optipng gifsicle webp

# PHP Modules
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && sync
RUN install-php-extensions bcmath curl exif gd intl imagick ldap mbstring memcached mysqli opcache pdo_mysql simplexml soap sockets redis xmlrpc xsl zip

# Install nodejs
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g yarn bower gulp pm2 svgo

# Composer
RUN \
    curl https://getcomposer.org/composer-1.phar --output composer-1.phar && \
    curl https://getcomposer.org/composer-2.phar --output composer-2.phar && \
    mv composer-1.phar /usr/local/bin/composer && \
    mv composer-2.phar /usr/local/bin/composer-2 && \
    chmod +x /usr/local/bin/composer-2 && \
    chmod +x /usr/local/bin/composer

RUN echo "Europe/Amsterdam" > /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata && \
    touch /var/log/cron.log

# Set default git user
RUN git config --global user.email "you@example.com" && \
git config --global user.name "Your Name"

# Healthcheck
RUN apt-get install -y libfcgi0ldbl \
    && curl -sL https://raw.githubusercontent.com/renatomefi/php-fpm-healthcheck/master/php-fpm-healthcheck --output /usr/local/bin/php-fpm-healthcheck \
    && chmod +x /usr/local/bin/php-fpm-healthcheck
HEALTHCHECK CMD php-fpm-healthcheck || exit 1

RUN mkdir -p /app
VOLUME /app
WORKDIR /app
