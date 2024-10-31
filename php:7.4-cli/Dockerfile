FROM php:7.4-cli

ENV TIMEZONE 'Europe/London'
ENV COMPOSER_NO_INTERACTION=1
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV NVM_DIR=/root/.nvm
ENV NVM_VERSION v0.35.3

RUN apt-get update

# Required to add yarn package repository
RUN apt-get install -y apt-transport-https gnupg

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update && apt-get install -y \
        libbz2-dev \
        libsodium-dev \
        git \
        unzip \
        wget \
        libpng-dev \
        libgconf-2-4 \
        libfontconfig1 \
        chromium \
        xvfb \
        yarn \
        libzip-dev

RUN docker-php-ext-install -j$(nproc) \
        bcmath \
        bz2 \
        calendar \
        exif \
        gd \
        sodium \
        pcntl \
        pdo_mysql \
        shmop \
        sockets \
        sysvmsg \
        sysvsem \
        sysvshm \
        zip

RUN pecl install redis xdebug-2.8.0

RUN docker-php-ext-enable \
        redis \
        xdebug

RUN mkdir -p $NVM_DIR && \
    curl -o- https://raw.githubusercontent.com/creationix/nvm/${NVM_VERSION}/install.sh | bash

RUN . ~/.nvm/nvm.sh && \
        nvm install lts/carbon && \
        nvm alias default lts/carbon

COPY ./scripts ./scripts

RUN chmod +rx ./scripts/*.sh

RUN ./scripts/composer.sh

RUN composer global require laravel/vapor-cli --update-with-dependencies

ENTRYPOINT ["/scripts/start.sh"]

CMD ["php", "-a"]