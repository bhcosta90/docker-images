ARG IMAGE
FROM $IMAGE

ENV DOCKERIZE_VERSION v0.6.1

COPY entrypoint.sh /srv/commands.sh

#Default
RUN apk add bash supervisor openssl-dev $PHPIZE_DEPS autoconf g++ make && \
    docker-php-ext-install bcmath pcntl ctype fileinfo json tokenizer && \
    wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz && \
    tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz && \
    rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz

COPY supervisord.conf /etc/supervisord.conf
COPY supervisord /etc/supervisor/conf.d/

RUN chmod +x /srv/commands.sh

WORKDIR /var/www/html

# ENTRYPOINT [ "php-fpm" ]
ENTRYPOINT ["/srv/commands.sh"]