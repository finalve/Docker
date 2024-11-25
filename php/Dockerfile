# เลือก base image ที่มี PHP 8.1 และ Apache
FROM php:8.1-apache

# ติดตั้งโปรแกรมที่จำเป็นเพื่อให้ Laravel ทำงานได้
RUN apt-get update && \
    apt-get install -y libzip-dev zip unzip && \
    docker-php-ext-install pdo_mysql zip && \
    a2enmod rewrite

# Set up Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# ก๊อปปี้โค้ดของโปรเจค Laravel เข้าสู่ Docker container
COPY . /var/www/html

# ตั้งค่าโปรเจค Laravel
WORKDIR /var/www/html
RUN composer install
RUN php artisan key:generate
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# เปลี่ยนเป็น user www-data สำหรับเซิร์ฟเวอร์ Apache
USER www-data

# ปล่อย port 80 ให้ Docker container
EXPOSE 80

# รัน Apache server
CMD ["apache2-foreground"]