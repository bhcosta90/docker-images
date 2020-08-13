ARG IMAGE
FROM $IMAGE

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
    docker-php-ext-install bcmath pcntl ctype fileinfo tokenizer

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
