FROM ubuntu:20.04

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
    && echo "deb [signed-by=/usr/share/keyrings/ppa_ondrej_php.gpg] https://ppa.launchpadcontent.net/ondrej/php/ubuntu focal main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
    && apt-get update \
    && apt-get install -y php8.0-cli php8.0-common php8.0-amqp php8.0-bcmath php8.0-bz2 php8.0-curl php8.0-gd php8.0-gmp \
       php8.0-igbinary php8.0-imagick php8.0-intl php8.0-mbstring php8.0-mcrypt php8.0-memcached php8.0-mongodb php8.0-msgpack \
       php8.0-mysql php8.0-readline php8.0-redis php8.0-soap php8.0-sqlite3 php8.0-ssh2 php8.0-swoole php8.0-tidy \
       php8.0-xml php8.0-xmlrpc php8.0-xsl php8.0-yaml php8.0-zip php8.0-opcache php8.0-protobuf \
       mysql-client \
    && curl -sLS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer \
    && apt-get install -y mysql-client \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN setcap "cap_net_bind_service=+ep" /usr/bin/php8.0

COPY php.ini /etc/php/8.0/cli/conf.d/99_overrides.ini
