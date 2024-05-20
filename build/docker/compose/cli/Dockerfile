FROM php:8.3-apache

ENV TZ=America/Phoenix

RUN printf "date.timezone = America/Phoenix\n" >> /usr/local/etc/php/php.ini

COPY build/docker/apache.conf /etc/apache2/conf-enabled/zz_pcr_dev.conf
