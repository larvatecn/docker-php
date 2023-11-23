FROM ubuntu:22.04

LABEL maintainer="Larva Tech"

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=UTC

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
#    && sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list \
#    && apt-get update \
RUN apt-get update \
    && mkdir -p /etc/apt/keyrings \
    && apt-get install -y gnupg gosu curl ca-certificates zip unzip git supervisor sqlite3 libcap2-bin libpng-dev python2 dnsutils librsvg2-bin \
    && curl -sS 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x14aa40ec0831756756d7f66c4f4ea0aae5267a6c' | gpg --dearmor | tee /etc/apt/keyrings/ppa_ondrej_php.gpg > /dev/null \
    && echo "deb [signed-by=/etc/apt/keyrings/ppa_ondrej_php.gpg] https://ppa.launchpadcontent.net/ondrej/php/ubuntu jammy main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
    && apt-get update \
    && apt-get install -y php8.3-cli php8.3-common php8.3-amqp php8.3-bcmath php8.3-bz2 php8.3-curl php8.3-gd php8.3-gmp \
       php8.3-igbinary php8.3-imagick php8.3-intl php8.3-mbstring php8.3-mcrypt php8.3-memcached php8.3-mongodb php8.3-msgpack \
       php8.3-mysql php8.3-psr php8.3-readline php8.3-redis php8.3-soap php8.3-sqlite3 php8.3-ssh2 php8.3-tidy \
       php8.3-xml php8.3-xsl php8.3-zip php8.3-opcache \
       mysql-client \
    && curl -sLS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer \
    && apt-get update \
    && apt-get install -y mysql-client \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN setcap "cap_net_bind_service=+ep" /usr/bin/php8.3

COPY php.ini /etc/php/8.2/cli/conf.d/99_overrides.ini
