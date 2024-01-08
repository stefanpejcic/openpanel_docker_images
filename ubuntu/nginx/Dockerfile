FROM ubuntu:22.04

ENV TZ=UTC
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update && \
    apt-get install --no-install-recommends -y \
        ttyd \
        nginx \
        mysql-server \
        php8.2-fpm \
        php8.2-mysql \
        php8.2-curl \
        php8.2-gd \
        php8.2-mbstring \
        php8.2-xml \
        php8.2-xmlrpc \
        php8.2-soap \
        php8.2-intl \
        php8.2-zip \
        php8.2-bcmath \
        php8.2-calendar \
        php8.2-exif \
        php8.2-ftp \
        php8.2-ldap \
        php8.2-sockets \
        php8.2-sysvmsg \
        php8.2-sysvsem \
        php8.2-sysvshm \
        php8.2-tidy \
        php8.2-uuid \
        php8.2-opcache \
        php8.2-redis \
        php8.2-memcached \
        php8.2-imagick \
        curl \
        pwgen \
        zip \
        unzip \
        wget \
        cron \
        phpmyadmin \
        openssh-server \
        nodejs \
        npm \
        python3.9 && \
        apt-get clean && \
        apt-get autoremove -y && \
        rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


########## PM2 ##########
RUN npm install pm2 -g



########## MySQL ##########
RUN sed -i 's/localhost/127.0.0.1/g' /etc/mysql/mysql.conf.d/mysqld.cnf



########## PORTOVI ##########
EXPOSE 22 80 3306 7681 8080



########## NGINX ##########
RUN sed -i 's/# server_names_hash_bucket_size 64;/server_names_hash_bucket_size 64;/' /etc/nginx/nginx.conf



########## SSH ##########
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile


########## PHPMYADMIN ##########
RUN sed -i 's/localhost/127.0.0.1/g' /etc/phpmyadmin/config-db.php

RUN sed -i 's/^upload_max_filesize = .*/upload_max_filesize = 1024M/' /etc/php/8.2/cli/php.ini
RUN sed -i 's/^max_input_time = .*/max_input_time = 600/' /etc/php/8.2/cli/php.ini
RUN sed -i 's/^memory_limit = .*/memory_limit = -1/' /etc/php/8.2/cli/php.ini
RUN sed -i 's/^post_max_size = .*/post_max_size = 1024M/' /etc/php/8.2/cli/php.ini
RUN sed -i 's/^max_execution_time = .*/max_execution_time = 600/' /etc/php/8.2/cli/php.ini

COPY config.inc.php /etc/phpmyadmin/
COPY pma.php /usr/share/phpmyadmin/pma.php




########## PHP-FPM ##########

# 8.2
RUN sed -i 's/^upload_max_filesize = .*/upload_max_filesize = 1024M/' /etc/php/8.2/fpm/php.ini
RUN sed -i 's/^max_input_time = .*/max_input_time = 600/' /etc/php/8.2/fpm/php.ini
RUN sed -i 's/^memory_limit = .*/memory_limit = -1/' /etc/php/8.2/fpm/php.ini
RUN sed -i 's/^post_max_size = .*/post_max_size = 1024M/' /etc/php/8.2/fpm/php.ini
RUN sed -i 's/^max_execution_time = .*/max_execution_time = 600/' /etc/php/8.2/fpm/php.ini
RUN sed -i 's/^opcache.enable= .*/opcache.enable=1/' /etc/php/8.2/fpm/php.ini






########## WP-CLI ##########
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x wp-cli.phar && \
    mv wp-cli.phar /usr/local/bin/wp

########## cleanup ##########
RUN rm -rf /var/cache/apk/* /tmp/* /var/tmp/*



########## docker run entrypoint  ##########
COPY entrypoint.sh /etc/entrypoint.sh
RUN chmod +x /etc/entrypoint.sh
CMD ["/bin/sh", "-c", "/etc/entrypoint.sh ; tail -f /dev/null"]
