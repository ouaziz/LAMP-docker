FROM php:8.0-fpm

# Copy composer.lock and composer.json
COPY script/saas-starter-kit/composer.lock script/saas-starter-kit/composer.json /var/www/

# Set working directory
WORKDIR /var/www


ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions mbstring pdo_mysql zip exif pcntl gd


# Install dependencies
RUN apt-get update -y
RUN apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    libzip-dev \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
# RUN docker-php-ext-install pdo_mysql 
# RUN docker-php-ext-install mbstring
# RUN docker-php-ext-install zip
# RUN docker-php-ext-install exif
# RUN docker-php-ext-install pcntl
# RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
# RUN docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY ./script/saas-starter-kit /var/www

# Copy existing application directory permissions
COPY --chown=www:www ./script/saas-starter-kit /var/www

# Change current user to www
USER www

RUN composer install
# RUN php artisan migrate
# RUN php artisan db:seed
# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]