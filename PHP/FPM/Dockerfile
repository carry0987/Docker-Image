ARG PHP_VERSION=8.2.12

# Use multi-stage builds to reduce final image size
# Stage 1: Build stage
FROM --platform=$BUILDPLATFORM php:${PHP_VERSION}-fpm as builder

RUN apt-get update && apt-get install -y --no-install-recommends \
        libvips-dev libvips-tools \
        imagemagick libmagickwand-6.q16-dev \
        build-essential zlib1g-dev libwebp-dev libpng-dev libjpeg-dev libfreetype6-dev \
        libzip-dev libonig-dev libcurl4-openssl-dev libssl-dev libxml2-dev unzip zip \
    && printf "\n" | pecl install vips imagick redis \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg --with-webp \
    && docker-php-ext-install pdo_mysql bcmath mysqli exif gd curl mbstring zip xml fileinfo \
    && docker-php-ext-enable vips imagick redis opcache \
    # Clean up to reduce layer size
    && pecl clear-cache \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Stage 2: Production stage
FROM --platform=$BUILDPLATFORM php:${PHP_VERSION}-fpm

# Copy installed extensions and their dependencies
COPY --from=builder /usr/local/etc/php /usr/local/etc/php
COPY --from=builder /usr/local/lib/php /usr/local/lib/php
COPY --from=builder /usr/local/bin /usr/local/bin
COPY --from=builder /usr/local/sbin /usr/local/sbin

# Install runtime library dependencies for PHP extensions
RUN apt-get update && apt-get install -y --no-install-recommends \
        libvips42 \
        libmagickwand-6.q16-6 \
        libwebp7 \
        libpng16-16 \
        libjpeg62-turbo \
        libfreetype6 \
        libzip4 \
    && apt-get purge --auto-remove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Composer in the final stage to avoid carrying over build dependencies
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
