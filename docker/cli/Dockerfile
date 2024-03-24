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
        /src \
        /etc/php \
        /var/lock \
        /var/cache \
        /var/lib/php \
    && cp -r \
       /etc/php-z/* \
       /etc/php/${PHP_VERSION} \
    && ln -sf \
        /etc/php/${PHP_VERSION}/90-php.ini \
        /etc/php/${PHP_VERSION}/cli/conf.d/90-php.ini \
    && chmod +x \
        /docker-entrypoint \
    && chown rootless:rootless \
        /run \
        /src \
        /etc/php \
        /var/lock \
        /var/cache \
        /var/lib/php \
    && rm -rf \
        /src/* \
        /var/www/* \
        /var/lib/apt/lists/* \
        /etc/php-z

ENTRYPOINT ["/docker-entrypoint"]

WORKDIR /src

CMD ["php", "-a"]

USER rootless
