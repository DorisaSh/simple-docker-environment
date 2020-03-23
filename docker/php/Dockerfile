FROM php:7.2-apache-stretch
LABEL maintainer="Dorisa Shehi <doris.shehi2@gmail.com>"

# Configure PHP
RUN apt-get update
RUN apt-get install -y --no-install-recommends \
        wget \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libpq-dev \
        zlib1g-dev \
# Install required 3rd party tools
        graphicsmagick 
# Configure extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) mysqli soap gd zip opcache intl pgsql pdo_pgsql 
RUN echo 'always_populate_raw_post_data = -1\nmax_execution_time = 240\nmax_input_vars = 1500\nupload_max_filesize = 32M\npost_max_size = 32M' > /usr/local/etc/php/conf.d/typo3.ini 
# Configure Apache as needed
  
RUN apt-get clean
RUN apt-get -y purge \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev
RUN rm -rf /var/lib/apt/lists/* /usr/src/*

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout /etc/ssl/private/ssl-cert-snakeoil.key \
        -out /etc/ssl/certs/ssl-cert-snakeoil.pem \
        -subj "/C=AL/ST=Albania/L=Shkoder/O=COMPANY/OU=SHPK/CN=Docker"

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN a2enmod rewrite
RUN a2ensite default-ssl
RUN a2enmod ssl