FROM php:8.2-apache

ARG user=laraveluser
ARG uid=1000

# Usar root para todas las instalaciones
USER root

# Instalar dependencias del sistema y extensiones PHP
RUN apt-get update && apt-get install -y \
    libpq-dev \
    libldap2-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libwebp-dev \
    libxpm-dev \
    libzip-dev \
    unzip \
    git \
    curl \
    gnupg2 \
    ca-certificates \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp --with-xpm \
    && docker-php-ext-install \
        pgsql \
        pdo_pgsql \
        ldap \
        gd \
        zip


# Instalar Composer desde imagen oficial
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Crear usuario personalizado
RUN groupadd -g ${uid} ${user} \
    && useradd -m -u ${uid} -g ${user} -s /bin/bash ${user} \
    && mkdir -p /home/${user}/.composer \
    && chown -R ${user}:${user} /home/${user}

# Habilitar mod_rewrite
RUN a2enmod rewrite \
 && a2enmod headers


# Establecer zona horaria (opcional)
ENV TZ=America/Mexico_City

RUN chown -R www-data:www-data /var/www/html

RUN sed -i 's|DocumentRoot /var/www/html|DocumentRoot /var/www/html/public|' /etc/apache2/sites-available/000-default.conf

# Directorio de trabajo
WORKDIR /var/www/html

# Usar el usuario personalizado
USER ${user}
