FROM php:fpm-jessie

LABEL MAINTAINER Jordan Wamser<jwamser@redpandacoding.com>

ARG buildtime_variable=development
ENV PHP_ENV=$buildtime_variable

# install php 72
RUN apt-get update
RUN apt-get install -y  apt-transport-https lsb-release ca-certificates
# wget
# RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
# RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
# RUN apt-get update
RUN apt-get install -y php-pear
# php7.2 php7.2-fpm 
RUN pecl channel-update pecl.php.net

RUN apt-get install -y curl
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install -y msodbcsql mssql-tools
# RUN ACCEPT_EULA=Y apt-get install mssql-tools
# RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
# RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
# RUN source ~/.bashrc
# # optional: for unixODBC development headers
RUN apt-get install -y unixodbc-dev

RUN pear config-set php_ini `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"` system

RUN pecl install sqlsrv && \
pecl install pdo_sqlsrv && \
docker-php-ext-enable sqlsrv pdo_sqlsrv

COPY ./phpfpm/ini/php.ini-${PHP_ENV} /usr/local/etc/php/php.ini

RUN echo extension=sqlsrv.so > /usr/local/etc/php/conf.d/sqlsrv.ini
RUN echo extension=pdo_sqlsrv.so > /usr/local/etc/php/conf.d/pdo_sqlsrv.ini

# EXPOSE 8080
# WORKDIR /app
# CMD ["php", "-S", "0.0.0.0:8080"]

# RUN ln -s /usr/bin/php-fpm7.2 /bin/php-fpm

# RUN useradd -ms /bin/bash web
# USER web

# ENTRYPOINT ["/usr/bin"]
##<autogenerated>##
# CMD ["bash"]
##</autogenerated>##

