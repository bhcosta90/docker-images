FROM php:7.4-fpm-alpine

COPY supervisord.conf /etc/
COPY entrypoint.sh /tmp/commands.sh

#Default
RUN apk add bash supervisor

WORKDIR /var/www/html

# ENTRYPOINT [ "php-fpm" ]
ENTRYPOINT ["/tmp/commands.sh"]