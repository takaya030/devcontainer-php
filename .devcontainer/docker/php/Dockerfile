FROM node:22-slim as node_builder

WORKDIR /app

RUN npm install -g npm@latest

FROM php:8.4-fpm

# install libs
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    zip \
    unzip \
    curl \
    git \
    && docker-php-ext-install pdo_mysql mbstring bcmath gd \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# copy node.js & npm
COPY --from=node_builder /usr/local/bin/node /usr/local/bin/node
COPY --from=node_builder /usr/local/bin/npm /usr/local/bin/npm
COPY --from=node_builder /usr/local/lib/node_modules /usr/local/lib/node_modules

# 既存の npm のリンクがない場合のみ作成
RUN [ ! -f /usr/local/bin/npm ] || rm -f /usr/local/bin/npm && \
    ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm

# 既に www-data ユーザーが存在するか確認し、存在しない場合のみ作成
RUN id www-data || useradd -m -d /home/www-data -s /bin/bash www-data

# www-data のデフォルトシェルを /bin/bash に変更
RUN usermod -s /bin/bash www-data

# www-data の権限を変更
RUN chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html

# www-data ユーザーでコンテナを実行
USER www-data

WORKDIR /var/www/html
