FROM php:7.4.33-fpm

ARG user
ARG uid

# ARG NAME=mundo
# RUN echo "Hola ${NAME}!"
# CMD echo "Hola ${NAME}!"

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip 

RUN apt-get update && \
        apt-get install -y \
            libzip-dev \
            && docker-php-ext-install zip

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get install -y nodejs

RUN chmod +x /home

RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user
WORKDIR /var/www

USER $user
