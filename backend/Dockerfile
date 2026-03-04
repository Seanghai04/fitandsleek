
# ប្រើ PHP 8.2 ជាមួយ Apache
FROM php:8.2-apache

# ដំឡើង System Dependencies ដែលចាំបាច់
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    libzip-dev \
    unzip \
    libpq-dev \
    && docker-php-ext-configure medical --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_pgsql pgsql zip gd

# បើក Apache mod_rewrite សម្រាប់ Laravel
RUN a2enmod rewrite

# កំណត់ Working Directory
WORKDIR /var/www/html

# ចម្លងកូដទាំងអស់ចូលក្នុង Container
COPY . .

# ដំឡើង Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer install --no-dev --optimize-autoloader

# កំណត់សិទ្ធិ (Permissions) លើ Storage និង Cache
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache
RUN chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

# កំណត់ Port សម្រាប់ Apache (Render ប្រើ Port 80 ឬ $PORT)
ENV PORT=80
EXPOSE 80

# កំណត់ការកំណត់របស់ Apache ឱ្យរត់ទៅកាន់ /public
RUN sed -ri -e 's!/var/www/html!/var/www/html/public!g' /etc/apache2/sites-available/*.conf

# បញ្ជាឱ្យ Start Apache
CMD ["apache2-foreground"]
