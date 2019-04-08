FROM debian:stretch

MAINTAINER Jeremy Chaufourier <jeremy.chaufourier@gmail.com>

RUN apt-get update \
    && apt-get install -y \
         curl \
         vim \
         wget \
         wkhtmltopdf \
         pngquant \
         jpegoptim \
         openssl \
         apache2 \
         php \
         php-cli \
         php-curl \
         php-gd \
         php-geoip \
         php-imagick \
         php-mbstring \
         php-mcrypt \
         php-mysql \
         php-sqlite3 \
         php-xsl \
         php-xml \
         php-xdebug

ENV APACHE_LOG_DIR /var/log/apache2

COPY ./config/apache2/apache2.conf /etc/apache2/apache2.conf
COPY ./config/php/7.0/php.ini /etc/php7/apache2/php.ini

ADD ./config/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default.conf
ADD ./config/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

RUN mkdir /etc/apache2/ssl

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -subj "/C=FR/ST=HB/L=Local/O=Dev/CN=localhost" \
  -keyout "/etc/apache2/ssl/server.key" \
  -out "/etc/apache2/ssl/server.crt"

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
RUN chmod +x wp-cli.phar
RUN mv wp-cli.phar /usr/local/bin/wp

RUN curl -s https://getcomposer.org/installer | php
RUN chmod +x composer.phar
RUN mv composer.phar /usr/local/bin/composer

RUN a2enmod rewrite
RUN a2enmod ssl
RUN a2enmod headers
RUN a2ensite default-ssl

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
