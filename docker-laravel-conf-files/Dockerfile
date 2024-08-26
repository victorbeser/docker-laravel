# Use a imagem oficial do PHP 8.2 com Apache
FROM php:8.2-apache

# Instale as extensões PHP necessárias e outras dependências
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libpq-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim unzip git curl \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd pdo_mysql mbstring xml zip pdo_pgsql \
    && a2enmod rewrite

# Instale o Composer globalmente
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Defina o diretório de trabalho
WORKDIR /var/www/html/laravel_w_pgsql_3

# Copie os arquivos do Laravel para o container
COPY . /var/www/html/laravel_w_pgsql_3

# Dê permissões apropriadas às pastas de armazenamento e cache do Laravel
RUN chown -R www-data:www-data /var/www/html/laravel_w_pgsql_3/storage /var/www/html/laravel_w_pgsql_3/bootstrap/cache
RUN chmod -R 775 /var/www/html/laravel_w_pgsql_3/storage /var/www/html/laravel_w_pgsql_3/bootstrap/cache

# Remova o diretório vendor para garantir um ambiente limpo
RUN rm -rf /var/www/html/laravel_w_pgsql_3/vendor

# Verifique as dependências de plataforma do Composer
RUN composer check-platform-reqs

# Instale as dependências do Laravel
RUN composer install --no-cache --optimize-autoloader --no-dev --verbose

# Copie o arquivo de configuração do Apache
COPY .docker/vhost.conf /etc/apache2/sites-available/000-default.conf

# Exponha a porta 80 para o Apache
EXPOSE 80
