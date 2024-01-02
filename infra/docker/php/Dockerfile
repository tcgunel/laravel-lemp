FROM php:8.1.3-fpm
LABEL maintainer="ucan-lab <yes@u-can.pro>"
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

ARG USER_NAME
ARG USER_ID

# timezone environment
ENV TZ=UTC \
  # locale
  LANG=en_US.UTF-8 \
  LANGUAGE=en_US:en \
  LC_ALL=en_US.UTF-8 \
  # composer environment
  COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer \
  USER_NAME=${USER_NAME?usernamenotset} \
  USER_ID=${USER_ID?useridnotset}

COPY --from=composer:2.1 /usr/bin/composer /usr/bin/composer

RUN apt-get update && \
  apt-get -y install git  \
    libicu-dev libonig-dev openssl libzip-dev libcurl4-openssl-dev pkg-config libssl-dev unzip locales \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev libxml2-dev build-essential g++ python2 curl libnotify-bin \
    libxslt-dev libpq-dev --fix-missing && \
  apt-get clean && \

  pecl install mongodb && \

  rm -rf /var/lib/apt/lists/* && \
  locale-gen en_US.UTF-8 && \
  localedef -f UTF-8 -i en_US en_US.UTF-8 && \
  mkdir /var/run/php-fpm && \

  docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql && \
  docker-php-ext-install intl pdo pdo_mysql pdo_pgsql pgsql zip bcmath exif soap xsl && \
  pecl install xdebug && \
  docker-php-ext-enable xdebug && \
  docker-php-ext-configure gd --with-freetype --with-jpeg && \
  docker-php-ext-install -j$(nproc) gd && \
  pecl install apfd && \
  docker-php-ext-enable apfd && \

  composer config -g process-timeout 3600 && \
  composer config -g repos.packagist composer https://packagist.org

RUN pecl install -o -f redis && rm -rf /tmp/pear && docker-php-ext-enable redis

RUN echo extension=mongodb.so >> /usr/local/etc/php/conf.d/mongodb.ini

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash && \
    export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
    nvm install 18

COPY ./infra/docker/php/php-fpm.d/zzz-www.conf /usr/local/etc/php-fpm.d/zzz-www.conf
COPY ./infra/docker/php/php.ini /usr/local/etc/php/php.ini

RUN usermod --non-unique --uid 1000 www-data

RUN useradd -rm --non-unique -d /home/${USER_NAME} -s /bin/bash -g root -G sudo -u ${USER_ID} ${USER_NAME}

WORKDIR /home/${USER_NAME}
