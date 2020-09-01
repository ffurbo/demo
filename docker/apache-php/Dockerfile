FROM php:7.4-apache

RUN apt-get update \
    && apt-get -y install \
        unzip \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libicu-dev \
        libmagickwand-dev \
        git \
        wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#        gnupg2 && \
#    apt-key update && \
#    apt-get update && \
#    apt-get -y install \
#            default-mysql-client \
#        --no-install-recommends && \
#        apt-get clean && \
#        rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*



RUN docker-php-ext-configure gd && \
    docker-php-ext-install \
        pdo_mysql \
        gd \
        intl


# Install PECL extensions
# see http://stackoverflow.com/a/8154466/291573) for usage of `printf`
RUN printf "\n" | pecl install \
        imagick \
        memcache && \
    docker-php-ext-enable \
        imagick \
        memcache

# Environment settings
ENV PHP_USER_ID=33 \
    PHP_ENABLE_XDEBUG=0 \
    PATH=/project:/root/.composer/vendor/bin:$PATH \
    TERM=linux \
#    VERSION_PRESTISSIMO_PLUGIN=^0.3.7 \
    COMPOSER_ALLOW_SUPERUSER=1

# Add configuration files
COPY image-files/ /

# Enable mod_rewrite for images with apache
RUN if command -v a2enmod >/dev/null 2>&1; then \
        a2enmod rewrite headers \
    ;fi

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer.phar \
        --install-dir=/usr/local/bin && \
    composer.phar clear-cache

RUN wget https://get.symfony.com/cli/installer -O - | bash && \
        mv /root/.symfony/bin/symfony /usr/local/bin/symfony

WORKDIR /project

