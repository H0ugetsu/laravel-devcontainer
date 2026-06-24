FROM php:8.5-fpm

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    unzip \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    nodejs \
    npm \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install \
    pdo_mysql \
    mbstring \
    xml \
    zip \
    bcmath

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
COPY php/php.ini /usr/local/etc/php/

RUN groupadd --gid 1000 vscode \
    && useradd --uid 1000 --gid vscode --shell /bin/bash --create-home vscode \
    && usermod -aG www-data vscode

WORKDIR /var/www/html
RUN chown -R vscode:vscode /var/www/html

USER vscode

