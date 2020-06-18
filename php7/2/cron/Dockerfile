FROM php:7.2-fpm-alpine

#Default
RUN apk add bash supervisor openssl-dev $PHPIZE_DEPS autoconf g++ make

COPY supervisord.conf /etc/supervisord.conf
COPY entrypoint.sh /srv/commands.sh

RUN chmod +x /srv/commands.sh

WORKDIR /var/www/html

# ENTRYPOINT [ "php-fpm" ]
ENTRYPOINT ["/srv/commands.sh"]