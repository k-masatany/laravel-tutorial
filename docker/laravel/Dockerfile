FROM alpine:3.8

RUN apk --no-cache add apache2 php7-apache2 \
    php7 php7-fpm php7-json php7-zlib php7-xml php7-xmlwriter php7-pdo php7-phar \
    php7-openssl php7-curl php7-pdo_pgsql \
    php7-gd php7-ctype php7-dom \
    php7-mbstring php7-session php7-tokenizer && \
    mkdir -p /run/apache2/ && \
    sed -i -e 's/\/var\/www\/localhost\/htdocs/\/var\/www\/html\/public/g' /etc/apache2/httpd.conf && \
    ln -sf /dev/stdout /var/log/apache2/access.log && \
    ln -sf /dev/stderr /var/log/apache2/error.log && \
    ln -sf /usr/bin/php7 /usr/bin/php

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/bin/composer

WORKDIR /var/www/html
EXPOSE 80

CMD [ "httpd", "-D", "FOREGROUND" ]
