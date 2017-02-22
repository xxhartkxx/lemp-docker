FROM php:5.6-fpm

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install -y libmemcached-dev zlib1g-dev supervisor postfix rsyslog

RUN pecl install memcached

RUN docker-php-ext-enable memcached
RUN docker-php-ext-install pdo_mysql

ADD supervisord.conf /etc/supervisor/supervisord.conf
ADD rsyslog.conf /etc/rsyslog.conf

RUN cp /etc/resolv.conf /var/spool/postfix/etc/
RUN cp /etc/services /var/spool/postfix/etc/
RUN cp /usr/share/zoneinfo/Japan /var/spool/postfix/etc/localtime

CMD ["/usr/bin/supervisord"]
