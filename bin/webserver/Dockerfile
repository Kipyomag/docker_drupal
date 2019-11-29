FROM php:5.6-apache

RUN apt-get -y update && apt-get upgrade -y

# Install tools && libraries
RUN apt-get -y install --fix-missing apt-utils nano wget dialog \
    build-essential git curl libcurl3 libcurl3-dev zip \
    libmcrypt-dev libsqlite3-dev libsqlite3-0 mysql-client \
    zlib1g-dev libicu-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    xfonts-utils xfonts-base xfonts-75dpi fontconfig libxrender1  \
    && rm -rf /var/lib/apt/lists/*

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# PHP5 Extensions
RUN docker-php-ext-install curl \
    && docker-php-ext-install tokenizer \
    && docker-php-ext-install json \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_sqlite \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && pecl install xdebug-2.5.5 && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/php.ini

# Wkhtmltopdf
RUN wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb
RUN dpkg -i wkhtmltox_0.12.5-1.stretch_amd64.deb
RUN rm wkhtmltox_0.12.5-1.stretch_amd64.deb

# Temporary file dir
RUN mkdir -p /data/www/root/internet/tmp
RUN chmod 777 /data/www/root/internet/tmp
# install drush 8
# https://github.com/drush-ops/drush/releases/download/8.3.0/drush.phar
RUN set -ex; \
    wget https://github.com/drush-ops/drush/releases/download/8.3.0/drush.phar; \
    chmod +x drush.phar; \
    mv drush.phar /usr/local/bin/drush;
#    drush8 init;

# Enable apache modules
RUN a2enmod rewrite headers

EXPOSE 80

ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]