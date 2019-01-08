FROM php:7.1-apache

LABEL maintainer="durga shankar gupta <dsguptas@gmail.com>"
LABEL php_version="7.1"
LABEL magento_version="2.2.7"
LABEL description="Magento 2.2.7 with PHP 7.1"


ENV COMPOSER_HOME /var/www/.composer/
ENV INSTALL_DIR /var/www/html
ENV MAGENTO_VERSION 2.2.6


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
    libssl-dev \
    libedit-dev \
    apt-utils \
    libedit2 \
    redis-tools \
    mysql-client \
    vim \
    wget \
    lynx \
    bash-completion \
    --no-install-recommends \
    && docker-php-ext-install -j$(nproc) iconv mcrypt pdo_mysql pdo_pgsql pdo_sqlite zip bcmath intl xsl soap opcache curl\
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd 
    

RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/log/supervisor


# Install Node.js and npm
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -
RUN apt-get install -y nodejs 
RUN npm install -g grunt-cli


#add default magento source code
RUN cd /tmp && \ 
  curl https://codeload.github.com/magento/magento2/tar.gz/$MAGENTO_VERSION -o $MAGENTO_VERSION.tar.gz && \
  tar xvf $MAGENTO_VERSION.tar.gz && \
  mv magento2-$MAGENTO_VERSION/* magento2-$MAGENTO_VERSION/.htaccess $INSTALL_DIR

RUN ls -ltr $INSTALL_DIR

RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/bin --filename=composer


RUN wget https://files.magerun.net/n98-magerun2.phar \
	&& chmod +x ./n98-magerun2.phar \
	&& mv ./n98-magerun2.phar /usr/local/bin/

RUN cd $INSTALL_DIR \
    && find . -type d -exec chmod 770 {} \; \
    && find . -type f -exec chmod 660 {} \; \
    && chmod u+x bin/magento \
    && chmod g+s . -R 
    


RUN a2enmod rewrite 
RUN echo "memory_limit=2048M" > /usr/local/etc/php/conf.d/memory-limit.ini
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add cron job
# ADD crontab /etc/cron.d/magento2-cron
# RUN chmod 0644 /etc/cron.d/magento2-cron \
#     && crontab -u www-data /etc/cron.d/magento2-cron

env APACHE_RUN_USER    www-data
env APACHE_RUN_GROUP   www-data
env APACHE_PID_FILE    /var/run/apache2.pid
env APACHE_RUN_DIR     /var/run/apache2
env APACHE_LOCK_DIR    /var/lock/apache2
env APACHE_LOG_DIR     /var/log/apache2
env LANG               C

RUN chown -R www-data:www-data $INSTALL_DIR

VOLUME /var/www/html
WORKDIR /var/www/html

CMD ["/usr/bin/supervisord"]

