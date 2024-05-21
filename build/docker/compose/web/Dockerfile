FROM php:8.3-apache

########################################################################################################################
# Environment variables and arguments
########################################################################################################################

# Project title
ENV SSC_PROJECT=PCR

ENV LC_ALL=C.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8

ENV TZ=America/Phoenix

RUN printf "date.timezone = America/Phoenix\n" >> /usr/local/etc/php/php.ini

COPY build/docker/apache.conf /etc/apache2/conf-enabled/zz_pcr_dev.conf

########################################################################################################################
# Set terminal defaults
########################################################################################################################

# Default to BASH in terminals.
RUN ln -sf /bin/bash /bin/sh
ENV ENV ~/.profile

########################################################################################################################
## Install the apt utils and setup our locales
########################################################################################################################

RUN apt-get update && apt install -y apt-utils locales locales-all
RUN echo "${LANG} UTF-8" >> /etc/locale.gen && locale-gen

########################################################################################################################
# Installers and install utils
########################################################################################################################

# Needed for cURL
RUN apt-get update && apt-get install -y gnupg2

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update && apt-get install -y yarn

########################################################################################################################
# Other global utilities useful in terminals etc
########################################################################################################################

RUN apt-get update && apt-get install -y vim

# RAR - always handy.
RUN curl -L -o /tmp/rarlinux-x64-621.tar.gz https://www.win-rar.com/fileadmin/winrar-versions/rarlinux-x64-621.tar.gz \
    && gunzip /tmp/rarlinux-x64-621.tar.gz \
    && tar -xf /tmp/rarlinux-x64-621.tar -C /tmp/ \
    && cp /tmp/rar/rar /usr/local/bin \
    && cp /tmp/rar/unrar /usr/local/bin \
    && chmod u+x /usr/local/bin/rar /usr/local/bin/unrar \
    && rm -fr /tmp/rarlinux-x64-621.tar \
    && rm -fr /tmp/rar

########################################################################################################################
# PHP Extensisons
########################################################################################################################

# Global packages
RUN apt-get update -y && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libldap2-dev libxml2-dev libicu-dev zlib1g-dev libpng-dev libmemcached-dev libzip-dev libc-client-dev libkrb5-dev libxslt1-dev libpspell-dev libonig-dev zip unzip

RUN pecl install memcached

# Install PDO
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_mysql

# Install zip libraries and extension
RUN docker-php-ext-install zip

# Install intl library and extension
RUN docker-php-ext-configure intl && docker-php-ext-install intl

# Install Memcached
RUN docker-php-ext-enable memcached

# Install LDAP
RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && docker-php-ext-install ldap

# Install MYSQLI
RUN docker-php-ext-install mysqli

# Install support for *Nix System V semaphores
RUN docker-php-ext-install sysvmsg
RUN docker-php-ext-install sysvsem
RUN docker-php-ext-install sysvshm

# Session library
RUN docker-php-ext-install session

# Multibyte string support
RUN docker-php-ext-install mbstring && docker-php-ext-enable mbstring

########################################################################################################################
# Cleanups
########################################################################################################################

RUN apt-get autoremove -y && apt-get clean y
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/*

########################################################################################################################
# Copy and run
########################################################################################################################

COPY . /srv/www
WORKDIR /srv/www
