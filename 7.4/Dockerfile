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
    && apt-get install -y php7.4-cli php7.4-common php7.4-amqp php7.4-bcmath php7.4-bz2 php7.4-curl php7.4-gd php7.4-gmp \
       php7.4-igbinary php7.4-imagick php7.4-intl php7.4-mbstring php7.4-mcrypt php7.4-memcached php7.4-mongodb php7.4-msgpack \
       php7.4-mysql php7.4-psr php7.4-readline php7.4-redis php7.4-soap php7.4-sqlite3 php7.4-ssh2 php7.4-swoole php7.4-tidy \
       php7.4-xml php7.4-xmlrpc php7.4-xsl php7.4-yaml php7.4-zip php7.4-opcache php7.4-protobuf \
       mysql-client \
    && curl -sLS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer \
    && apt-get install -y mysql-client \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN setcap "cap_net_bind_service=+ep" /usr/bin/php7.4

COPY php.ini /etc/php/7.4/cli/conf.d/99_overrides.ini
