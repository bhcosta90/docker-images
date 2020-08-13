ARG IMAGE

FROM $IMAGE

COPY entrypoint.sh /srv/commands.sh

#Default
RUN apk add bash supervisor openssl-dev $PHPIZE_DEPS autoconf g++ make && \
    docker-php-ext-install bcmath pcntl ctype fileinfo tokenizer

COPY supervisord.conf /etc/supervisord.conf

RUN chmod +x /srv/commands.sh

WORKDIR /var/www/html

# ENTRYPOINT [ "php-fpm" ]
ENTRYPOINT ["/srv/commands.sh"]