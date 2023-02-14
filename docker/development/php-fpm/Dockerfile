ARG PHP_VERSION

FROM php:${PHP_VERSION}-fpm

ARG user
ARG uid

###############################
# Install system dependencies #
###############################
RUN apt-get update
RUN apt-get install -y apt-utils \
    gnupg2 \
    git \
    curl \
    libzip-dev \
    zip \
    unzip
RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip

###########
# IMAGICK #
###########
ARG INSTALL_IMAGICK=false
RUN if [ ${INSTALL_IMAGICK} = true ]; then \
    docker-php-ext-enable imagick \
    && docker-php-ext-install sockets \
    && docker-php-source delete \
;fi

###############
# Compile PHP #
###############
ARG INSTALL_DEV=false
RUN if [ ${INSTALL_IMAGICK} = true ]; then \
    apt-get install -y libssl-dev \
    && libz-dev \
;fi


#####################################
# PDO_POSTGRESQL:
#####################################
ARG INSTALL_PDO_POSTGRESQL=false
RUN if [ ${INSTALL_PDO_POSTGRESQL} = true ]; then \
    apt-get install -y libpq-dev \
    # Install pdo_pgsql
    && docker-php-ext-install pdo pdo_pgsql pgsql \
;fi

#####################################
# xdebug:
#####################################
ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    # Install the xdebug
    pecl install xdebug \
    docker-php-ext-enable xdebug \
    COPY ./php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini \
;fi


#####################################
# composer:
#####################################
ARG INSTALL_COMPOSER=false
RUN if [ ${INSTALL_COMPOSER} = true ]; then \
    # Install the composer
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
;fi


#####################################
# Swoole:
#####################################
ARG INSTALL_SWOOLE=false
RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    # Install swoole and requirements
    apt-get install -y --no-install-recommends libssl-dev \
 && pecl install swoole \
 && docker-php-ext-enable swoole \
;fi

#####################################
# Redis:
#####################################
ARG INSTALL_REDIS=false
RUN if [ ${INSTALL_REDIS} = true ]; then \
    # Install redis and requirements
    pecl install redis \
 && docker-php-ext-enable redis \
;fi

#####################################
# BcMath:
#####################################
ARG INSTALL_BCMATH=false
RUN if [ ${INSTALL_BCMATH} = true ]; then \
    docker-php-ext-install bcmath \
;fi

#####################################
# Nodejs:
#####################################
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get install -y nodejs


# Clear package lists
RUN rm -rf /var/lib/apt/lists/*

# Permissions
RUN chown -R root:www-data /var/www
RUN chmod u+rwx,g+rx,o+rx /var/www
RUN find /var/www -type d -exec chmod u+rwx,g+rx,o+rx {} +
RUN find /var/www -type f -exec chmod u+rw,g+rw,o+r {} +


WORKDIR /var/www