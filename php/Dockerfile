FROM php:8.0-fpm-alpine3.13
ARG APP_NAME
ARG APP_ROOT
ARG MYSQL_DATABASE
ARG MYSQL_USER
ARG MYSQL_PASSWORD
ARG MYSQL_ROOT_PASSWORD
ARG MAIL_FROM_ADDRESS

#composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME "/opt/composer"
ENV PATH "$PATH:/opt/composer/vendor/bin"
COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer

RUN apk update && apk add \
        zip \
        unzip \
        freetype-dev \
        libjpeg-turbo-dev \
        zlib-dev \
        openssl-dev \
        bzip2-dev \
        readline-dev \
        sqlite-dev \
        libffi-dev \
        mlocate \
        autoconf \
        bison \
        alpine-sdk \
        re2c \
        libxml2-dev \
        pkgconf \
        curl-dev openssl-dev \
        libpng-dev \
        oniguruma-dev \
        postgresql-dev \
        libzip-dev \
        jpeg-dev \
        zstd \
        zstd-dev \
        vim \
        git \
        openssh \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo_mysql opcache \
    && pecl install zstd \
    && pecl install igbinary \
    && printf "yes\nno\nyes\n" | pecl install redis \
    && pecl install xdebug \
    && docker-php-ext-enable zstd igbinary redis xdebug

RUN mkdir /var/command
WORKDIR /var/command
COPY ./files/laravel.gitignore ./

RUN composer global require "laravel/installer"

RUN echo -e " \
cd $APP_ROOT \n\
laravel -f new $APP_NAME --jet \n\
chmod -R 777 $APP_ROOT/$APP_NAME/storage \n\
cd $APP_ROOT/$APP_NAME \n\
composer require barryvdh/laravel-debugbar \n\
echo \"opcache.enable_cli=1\" > /usr/local/etc/php/conf.d/php.user.ini \n\
echo \"opcache.jit_buffer_size=128M\" > /usr/local/etc/php/conf.d/php.user.ini \n\
echo 'Created: /usr/local/etc/php/conf.d/php.user.ini' \n\
sed -i 's/APP_NAME=Laravel/APP_NAME=$APP_NAME/g' .env \n\
sed -i 's/DB_HOST=127.0.0.1/DB_HOST=mysql/g' .env \n\
sed -i 's/DB_DATABASE=$APP_NAME/DB_DATABASE=${MYSQL_DATABASE}/g' .env \n\
sed -i 's/DB_USERNAME=root/DB_USERNAME=${MYSQL_USER}/g' .env \n\
sed -i 's/DB_PASSWORD=/DB_PASSWORD=${MYSQL_PASSWORD}/g' .env \n\
sed -i 's/REDIS_HOST=127.0.0.1/REDIS_HOST=redis/g' .env \n\
sed -i 's/MAIL_USERNAME=null/MAIL_USERNAME=testuser/g' .env \n\
sed -i 's/MAIL_FROM_ADDRESS=null/MAIL_FROM_ADDRESS=${MAIL_FROM_ADDRESS}/g' .env \n\
sed -i 's/SESSION_DRIVER=database/SESSION_DRIVER=redis/g' .env \n\
sed -i 's/SESSION_LIFETIME=120/SESSION_LIFETIME=30/g' .env \n\
sed -i 's/CACHE_DRIVER=file/CACHE_DRIVER=redis/g' .env \n\
sed -i 's/BROADCAST_DRIVER=log/BROADCAST_DRIVER=redis/g' .env \n\
php artisan vendor:publish --tag=livewire:assets \n\
echo --Command end.-- \n\
echo --[Next].-- \n\
printf '\e[32;1m[%s] \e[36m%s\e\\\n[m' '(php container) Command' 'cd $APP_ROOT/$APP_NAME && php artisan migrate' \n\
printf '\e[32m[%s] \e[37m%s\e\\\n[m' '(php container) Command' 'Ctrl + d' \n\
printf '\e[32;1m[%s] \e[36m%s\e\\\n[m' '(host) Command' 'docker exec -it ${APP_NAME}-node ash' \n\
printf '\e[32;1m[%s] \e[36m%s\e\\\n[m' '(node container) Command' 'npm update && npm install && npm run dev' \n\
printf '\e[32m[%s] \e[37m%s\e\\\n[m' '(node container) Command' 'Ctrl + d' \n\
" >> appinit.sh
