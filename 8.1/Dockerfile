FROM ubuntu:22.04

LABEL maintainer="Larva Tech"

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=UTC

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
#    && sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
#    && apt-get update \
RUN apt-get update \
    && apt-get install -y gnupg curl ca-certificates zip unzip git supervisor sqlite3 libcap2-bin libpng-dev python2 \
    && curl -sS 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x14aa40ec0831756756d7f66c4f4ea0aae5267a6c' | gpg --dearmor | tee /usr/share/keyrings/ppa_ondrej_php.gpg > /dev/null \
    && echo "deb [signed-by=/usr/share/keyrings/ppa_ondrej_php.gpg] https://ppa.launchpadcontent.net/ondrej/php/ubuntu jammy main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
    && apt-get update \
    && apt-get install -y php8.1-cli php8.1-common php8.1-amqp php8.1-bcmath php8.1-bz2 php8.1-curl php8.1-gd php8.1-gmp \
       php8.1-igbinary php8.1-imagick php8.1-intl php8.1-mbstring php8.1-mcrypt php8.1-memcached php8.1-mongodb php8.1-msgpack \
       php8.1-mysql php8.1-readline php8.1-redis php8.1-soap php8.1-sqlite3 php8.1-ssh2 php8.1-swoole php8.1-tidy \
       php8.1-xml php8.1-xmlrpc php8.1-xsl php8.1-yaml php8.1-zip php8.1-opcache php8.1-protobuf \
       mysql-client \
    && curl -sLS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer \
    && apt-get install -y mysql-client \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN setcap "cap_net_bind_service=+ep" /usr/bin/php8.1

COPY php.ini /etc/php/8.1/cli/conf.d/99_overrides.ini

