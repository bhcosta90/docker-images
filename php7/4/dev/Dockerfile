FROM php:7.4-fpm-alpine

ENV DOCKERIZE_VERSION v0.6.1

RUN echo "worker_processes 1;
error_log stderr warn;
pid /run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include mime.types;
    default_type application/octet-stream;

    # Define custom log format to include reponse times
    log_format main_timed '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for" '
                          '$request_time $upstream_response_time $pipe $upstream_cache_status';

    access_log /dev/stdout main_timed;
    error_log /dev/stderr notice;

    keepalive_timeout 65;

    # Write temporary files to /tmp so they can be created as a non-privileged user
    client_body_temp_path /tmp/client_temp;
    proxy_temp_path /tmp/proxy_temp_path;
    fastcgi_temp_path /tmp/fastcgi_temp;
    uwsgi_temp_path /tmp/uwsgi_temp;
    scgi_temp_path /tmp/scgi_temp;

    # Default server definition
    server {
        listen [::]:8080 default_server;
        listen 80 default_server;
        server_name _;

        sendfile off;

        root /var/www/html/public;
        index index.php index.html;

        location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to index.php
            try_files $uri $uri/ /index.php?q=$uri&$args;
        }

        # Redirect server error pages to the static page /50x.html
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root /var/lib/nginx/html;
        }

        # Pass the PHP scripts to PHP-FPM listening on 127.0.0.1:9000
        location ~ \.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param SCRIPT_NAME $fastcgi_script_name;
            fastcgi_index index.php;
            include fastcgi_params;
        }

        location ~* \.(jpg|jpeg|gif|png|css|js|ico|xml)$ {
            expires 5d;
        }

        # Deny access to . files, for security
        location ~ /\. {
            log_not_found off;
            deny all;
        }

        # Allow fpm ping and status from localhost
        location ~ ^/(fpm-status|fpm-ping)$ {
            access_log off;
            allow 127.0.0.1;
            deny all;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
            fastcgi_pass 127.0.0.1:9000;
        }
    }

    # Include other server configs
    include /etc/nginx/conf.d/*.conf;
}" >> /etc/nginx/nginx.conf

#Default
RUN apk add bash shadow git nginx supervisor -u $PHPIZE_DEPS \
    && rm /etc/nginx/conf.d/default.conf \
    && curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && mkdir -p /var/www/html \
    && rm /var/cache/apk/* \
    && usermod -u 1000 www-data \
    && apk --update --no-cache add autoconf g++ make \
    && pecl install -f xdebug \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=172.17.0.2" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_log=/var/www/html/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "* * * * * php /var/www/html/artisan schedule:run >> /dev/null 2>&1" >> /etc/crontab.txt \
    && wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && docker-php-ext-install bcmath

WORKDIR /var/www/html

# Expose the port nginx is reachable on
EXPOSE 80

CMD ['nginx start; php-fpm']

# Let supervisord start nginx & php-fpm
# CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

# Configure a healthcheck to validate that everything is up&running
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
