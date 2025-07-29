# Stage 1: Build dengan Composer
FROM composer:2 AS builder

WORKDIR /app
COPY . /app
RUN composer install --no-dev --optimize-autoloader


# Stage 2: PHP dengan Laravel siap jalan
FROM php:8.2-cli-alpine

# Install ekstensi Laravel butuhkan
RUN apk add --no-cache bash curl libpng libjpeg-turbo libwebp freetype icu-dev oniguruma-dev libzip-dev zip unzip \
 && docker-php-ext-install pdo pdo_mysql mbstring zip

WORKDIR /var/www

# Salin dari stage builder
COPY --from=builder /app /var/www

# Copy .env kalau belum ada
RUN cp .env.example .env

# Jalankan key:generate dan migrate saat container start
CMD php artisan key:generate && \
    php artisan migrate --seed && \
    php artisan serve --host=0.0.0.0 --port=8001
