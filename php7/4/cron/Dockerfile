FROM php:7.4-fpm-alpine

COPY supervisord.conf /etc/
COPY entrypoint.sh /srv/commands.sh

#Default
<<<<<<< HEAD
RUN apk add bash supervisor
=======
RUN apk add bash supervisor && \
    chmod +x /srv/commands.sh
>>>>>>> parent of cfb248d... arrumando dockerfile

WORKDIR /var/www/html


# ENTRYPOINT [ "php-fpm" ]
ENTRYPOINT ["/srv/commands.sh"]