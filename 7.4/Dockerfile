FROM larvatech/ubuntu:20.04

LABEL maintainer="support@larva.com.cn"

WORKDIR /app

RUN apt-get update \
    && apt-get install -y --no-install-recommends --no-install-suggests gnupg curl ca-certificates zip unzip git supervisor sqlite3 libpng-dev \
    && apt-get install -y --no-install-recommends --no-install-suggests php7.4-cli php7.4-dev \
       php7.4-pgsql php7.4-sqlite3 php7.4-gd \
       php7.4-curl php7.4-memcached \
       php7.4-imap php7.4-mysql php7.4-mbstring \
       php7.4-xml php7.4-zip php7.4-bcmath php7.4-soap \
       php7.4-intl php7.4-readline php7.4-pcov \
       php7.4-msgpack php7.4-igbinary \
       php7.4-redis \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && apt-get update \
    && apt-get install -y nginx mysql-client \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log \
	&& {\
		echo "<?php"; \
		echo "phpinfo();"; \
	} > /usr/local/html/info.php \
	&& chmod 775 /usr/local/html/info.php \
	&& {\
		echo "echo \"PHP version: ${PHP_VERSION}\""; \
		echo "echo \"Nginx version: ${NGINX_VERSION}\""; \
	} > /root/.bashrc

COPY start-container /usr/local/bin/start-container
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY php.ini /etc/php/7.4/cli/conf.d/99-sail.ini
RUN chmod +x /usr/local/bin/start-container

EXPOSE 80

HEALTHCHECK --interval=5s --timeout=2s --retries=12 \
  CMD curl --silent --fail localhost:80/ping || exit 1

ENTRYPOINT ["start-container"]
