ARG IMAGE
FROM $IMAGE

ENV DOCKERIZE_VERSION v0.6.1

COPY entrypoint.sh /srv/commands.sh

#Default
RUN apk add bash shadow git nginx -u $PHPIZE_DEPS && \
    chmod +x /srv/commands.sh && \
    rm /etc/nginx/conf.d/default.conf && \
    curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    mkdir -p /var/www/html && \
    rm /var/cache/apk/* && \
    usermod -u 1000 www-data && \
    groupmod -g  1000 www-data && \
    apk --update --no-cache add autoconf g++ make && \
    pecl install -f xdebug && \
    docker-php-ext-enable xdebug && \
    echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/xdebug.ini && \
    #echo "xdebug.client_host=192.168.1.27" >> /usr/local/etc/php/conf.d/xdebug.ini && \
    echo "xdebug.client_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini && \
    echo "xdebug.start_with_request=1" >> /usr/local/etc/php/conf.d/xdebug.ini && \
    echo "xdebug.discover_client_host=0" >> /usr/local/etc/php/conf.d/xdebug.ini && \
    echo "xdebug.idekey=docker" >> /usr/local/etc/php/conf.d/xdebug.ini && \
    echo "error_reporting=E_ALL" >> /usr/local/etc/php/conf.d/config.ini && \
    echo "display_startup_errors=On" >> /usr/local/etc/php/conf.d/config.ini && \
    echo "display_errors=On" >> /usr/local/etc/php/conf.d/config.ini && \
    docker-php-ext-install bcmath pcntl ctype fileinfo json tokenizer && \
    wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz && \
    tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz && \
    rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz && \
    docker-php-ext-install pdo pdo_mysql
    
COPY nginx.conf /etc/nginx/nginx.conf

WORKDIR /var/www/html

# Expose the port nginx is reachable on
EXPOSE 80

# ENTRYPOINT [ "php-fpm" ]
ENTRYPOINT ["/srv/commands.sh"]

# Let supervisord start nginx & php-fpm
# CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

# Configure a healthcheck to validate that everything is up&running
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:80/fpm-ping
