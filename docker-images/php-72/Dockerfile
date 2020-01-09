FROM php:7.2-fpm

# Install dependencies
RUN apt-get update \
  && apt-get install -y \
    libfreetype6-dev \ 
    libicu-dev \ 
    libjpeg62-turbo-dev \ 
    libmcrypt-dev \ 
    libpng-dev \ 
    libxslt1-dev \
    libjpeg-dev \
    zlib1g-dev \
    g++ \
    libxml2-dev \
    libxslt-dev \
    vim \
    swaks \
    cron \
    msmtp

# Configure the gd library
RUN docker-php-ext-configure \
  gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/

# Install required PHP extensions
RUN docker-php-ext-install \
  dom \ 
  gd \ 
  intl \ 
  mbstring \ 
  pdo_mysql \ 
  xsl \ 
  zip \ 
  soap \ 
  bcmath

# Install lib RabbitMQ
RUN apt-get install -y \
        librabbitmq-dev \
        libssh-dev \
    && docker-php-ext-install \
        sockets \
    && pecl install amqp \
    && docker-php-ext-enable amqp

# Install Xdebug (but don't enable)
RUN pecl install -o -f xdebug

# Config libs
ADD etc/php-xdebug.ini /usr/local/etc/php/conf.d/zz-xdebug-settings.ini
ADD etc/mail.ini /usr/local/etc/php/conf.d/zz-mail.ini
ADD etc/.msmtprc /root/.msmtprc
ADD etc/php-fpm.ini /usr/local/etc/php/conf.d/zz-magento.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer

CMD ["php-fpm", "-F"]