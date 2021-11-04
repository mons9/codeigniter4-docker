FROM php:7.4-fpm

RUN apt-get update \
&& apt-get install -y \
git \
zip \
libonig-dev \
unzip \
libzip-dev \
curl \
vim

RUN apt-get install -y libicu-dev

RUN docker-php-ext-install intl
RUN docker-php-ext-configure intl
RUN docker-php-ext-install mysqli pdo pdo_mysql zip mbstring

RUN docker-php-ext-enable mysqli
RUN docker-php-ext-enable pdo
RUN docker-php-ext-enable pdo_mysql

RUN printf "#!/bin/bash\nmv /ci4 /var/www/html\ndocker-php-entrypoint php-fpm" > /startScript.sh
RUN chmod +x /startScript.sh

RUN cd /var/www/html

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN composer self-update --2
RUN composer create-project codeigniter4/appstarter ci4 v4.1.3
RUN chmod -R 0777 /var/www/html/ci4/writable

RUN mv ci4 /

RUN apt-get clean \
    && rm -r /var/lib/apt/lists/*

VOLUME ["/var/www/html"]
CMD ["bash", "/startScript.sh"]
