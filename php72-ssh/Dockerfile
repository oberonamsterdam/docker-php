FROM oberonamsterdam/php:7.2-fpm
MAINTAINER chin@oberon.nl

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

RUN apt-get install -y libssh2-1-dev

# PHP Configuration
RUN pecl install ssh2-alpha && docker-php-ext-enable ssh2

# Cleanup
RUN apt-get purge -y --auto-remove apt-utils libssh2-1-dev && \
    apt-get -y clean && \
    apt-get -y purge && \
    rm -rf /var/lib/apt/lists/* /tmp/*
