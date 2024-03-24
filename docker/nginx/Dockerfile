FROM debian:stable-slim

ARG UID='1000'
ARG GID='1000'
ARG PHP_VERSION='8.3'

ENV PHP_VERSION=${PHP_VERSION}

RUN set -eux; \
    groupadd \
      --gid $GID \
      rootless \
    && useradd \
      --uid $UID \
      --gid $GID \
      --shell /bin/bash \
      rootless

RUN set -eux; \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        software-properties-common \
        ca-certificates \
        lsb-release \
        curl \
    && curl -qfsSL https://packages.sury.org/php/apt.gpg \
        | gpg --dearmor -o /usr/share/keyrings/php.gpg \
    && echo \
        "deb [arch=$(dpkg --print-architecture) \
        signed-by=/usr/share/keyrings/php.gpg] \
        https://packages.sury.org/php/ \
        $(lsb_release -cs) main" \
        | tee /etc/apt/sources.list.d/php.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        supervisor \
        nginx \
        php${PHP_VERSION}-fpm \
        php${PHP_VERSION}-cli \
        php${PHP_VERSION}-common \
        php${PHP_VERSION}-bcmath \
        php${PHP_VERSION}-opcache \
        php${PHP_VERSION}-apcu \
        php${PHP_VERSION}-xdebug \
        php${PHP_VERSION}-curl \
        php${PHP_VERSION}-gd \
        php${PHP_VERSION}-intl \
        php${PHP_VERSION}-iconv \
        php${PHP_VERSION}-tokenizer \
        php${PHP_VERSION}-mbstring \
        php${PHP_VERSION}-mysql \
        php${PHP_VERSION}-sqlite3 \
        php${PHP_VERSION}-pgsql \
        php${PHP_VERSION}-redis \
        php${PHP_VERSION}-amqp \
        php${PHP_VERSION}-xml \
        php${PHP_VERSION}-xsl \
        php${PHP_VERSION}-oauth \
        php${PHP_VERSION}-ftp \
        php${PHP_VERSION}-soap \
        php${PHP_VERSION}-ldap \
        php${PHP_VERSION}-zip

COPY system .

RUN set -eux; \
    mkdir -p \
        /run \
        /etc/php \
        /var/lock \
        /var/cache \
        /var/lib/php \
        /var/lib/nginx \
        /var/www \
    && cp -r \
       /etc/php-z/* \
       /etc/php/${PHP_VERSION} \
    && ln -sf \
        /etc/php/${PHP_VERSION}/90-php.ini \
        /etc/php/${PHP_VERSION}/cli/conf.d/90-php.ini \
    && ln -sf \
        /etc/php/${PHP_VERSION}/90-php.ini \
        /etc/php/${PHP_VERSION}/fpm/conf.d/90-php.ini \
    && ln -sf \
       /usr/sbin/php-fpm${PHP_VERSION} \
       /usr/sbin/php-fpm \
    && chmod +x \
        /docker-entrypoint \
    && chown rootless:rootless \
        /run \
        /etc/php \
        /var/lock \
        /var/cache \
        /var/lib/php \
        /var/lib/nginx \
        /var/www \
    && rm -rf \
        /src/* \
        /var/www/* \
        /var/lib/apt/lists/* \
        /etc/php-z

ENTRYPOINT ["/docker-entrypoint"]

WORKDIR /var/www

EXPOSE 8080

STOPSIGNAL SIGQUIT

CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf"]

USER rootless
