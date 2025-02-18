FROM php:7.3-apache

WORKDIR /var/www/html

# Define o frontend como não interativo para evitar prompts
ENV DEBIAN_FRONTEND=noninteractive

# Atualiza os pacotes e instala dependências
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    wget \
    unzip \
    git \
    curl \
    libzip-dev \
    apt-utils \
    libonig-dev \
    supervisor \
    libxml2-dev \
    && docker-php-source extract

# Instala o Node.js e npm
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest

# Instala as extensões PHP necessárias
RUN docker-php-ext-install gd \
    && docker-php-ext-install zip pcntl pdo_mysql mbstring exif

# Habilita o mod_rewrite do Apache
RUN a2enmod rewrite

# Instala o Redis
RUN mkdir -p /usr/src/php/ext/redis; \
    curl -fsSL https://pecl.php.net/get/redis | tar xvz -C "/usr/src/php/ext/redis" --strip 1; \
    docker-php-ext-install redis;

# Instala o Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Configuração de permissões de usuário
RUN groupadd -g 1000 laravel \
    && useradd -u 1000 -ms /bin/bash -g laravel user

# Copia os arquivos do projeto
COPY --chown=user:laravel . /var/www

# Configuração do Supervisor
COPY ../php/supervisord.conf /etc/supervisor/supervisord.conf
COPY ../php/supervisor/ /etc/supervisor/conf.d/

# Muda para o usuário "user"
USER user

# Expõe a porta 9000
EXPOSE 9000
