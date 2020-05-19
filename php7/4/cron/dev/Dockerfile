FROM php:7.4-fpm-alpine

COPY supervisord.conf /etc/
COPY entrypoint.sh /tmp/commands.sh

#Default
<<<<<<< HEAD:php7/4/cron/dev/Dockerfile
RUN apk add bash supervisor && \
    chmod +x /tmp/commands.sh
=======
RUN apk add bash supervisor \
    && docker-php-ext-install bcmath
>>>>>>> parent of c2899d4... arrumando image:php7/4/cron/Dockerfile

WORKDIR /var/www/html


# ENTRYPOINT [ "php-fpm" ]
ENTRYPOINT ["/tmp/commands.sh"]