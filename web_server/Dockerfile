FROM php:7.1-apache

LABEL maintainer="durga shankar gupta <dsguptas@gmail.com>"
LABEL php_version="7.1"
LABEL magento_version="2.2.7"
LABEL description="Magento 2.2.7 with PHP 7.1"


ENV COMPOSER_HOME /var/www/.composer/


# RUN apt-get update \
#     && apt-get -y install apache2 nano mysql-client \
#     && a2enmod rewrite \
#     && a2enmod headers \
#     && export LANG=en_US.UTF-8 \
#     && apt-get update \
#     && apt-get install -y software-properties-common \
#     && LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php \
#     && apt-get update \
#     && apt-get -y install php7.1 php7.1-curl php7.1-intl php7.1-gd php7.1-dom php7.1-mcrypt php7.1-iconv php7.1-xsl php7.1-mbstring php7.1-ctype   php7.1-zip php7.1-pdo php7.1-xml php7.1-bz2 php7.1-calendar php7.1-exif php7.1-fileinfo php7.1-json php7.1-mysqli php7.1-mysql php7.1-posix php7.1-tokenizer php7.1-xmlwriter php7.1-xmlreader php7.1-phar php7.1-soap php7.1-mysql php7.1-fpm php7.1-bcmath libapache2-mod-php7.1 \
#     && sed -i -e"s/^memory_limit\s*=\s*128M/memory_limit = 2048M/" /etc/php/7.1/apache2/php.ini \
#     && rm /var/www/html/* \
#     && sed -i "s/None/all/g" /etc/apache2/apache2.conf \
# ##install supervisor and setup supervisord.conf file
#     && apt-get install -y supervisor \
#     && mkdir -p /var/log/supervisor

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libmemcached-dev \
    zlib1g-dev \
    libltdl7 \
    libltdl-dev \
    libpq-dev \
    libsqlite3-dev \
    libxslt1-dev \
    git \
    curl \
    libcurl3-dev \
    rsyslog \
    cron \
    supervisor \
    unzip \
    libicu-dev \
    gnupg2 \
    --no-install-recommends \
    && docker-php-ext-install -j$(nproc) iconv mcrypt pdo_mysql pdo_pgsql pdo_sqlite zip bcmath intl xsl soap curl\
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd 
    
    

RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/log/supervisor

RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/bin --filename=composer

# Install Node.js and npm
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install -y nodejs 
RUN npm install -g grunt-cli


RUN a2enmod rewrite 
RUN echo "memory_limit=2048M" > /usr/local/etc/php/conf.d/memory-limit.ini
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add cron job
ADD crontab /etc/cron.d/magento2-cron
RUN chmod 0644 /etc/cron.d/magento2-cron \
    && crontab -u www-data /etc/cron.d/magento2-cron


env APACHE_RUN_USER    www-data
env APACHE_RUN_GROUP   www-data
env APACHE_PID_FILE    /var/run/apache2.pid
env APACHE_RUN_DIR     /var/run/apache2
env APACHE_LOCK_DIR    /var/lock/apache2
env APACHE_LOG_DIR     /var/log/apache2
env LANG               C

WORKDIR /var/www/html

CMD ["/usr/bin/supervisord"]

