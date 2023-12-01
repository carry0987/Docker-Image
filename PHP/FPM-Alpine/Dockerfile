ARG PHP_VERSION

# Stage 1: Build stage
FROM --platform=$BUILDPLATFORM php:${PHP_VERSION}-fpm-alpine AS builder

# Install dependencies and PHP extensions
RUN apk update && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        vips-dev \
        imagemagick-dev \
        zlib-dev \
        libwebp-dev \
        libpng-dev \
        libjpeg-turbo-dev \
        freetype-dev \
        libzip-dev \
        oniguruma-dev \
        curl-dev \
        openssl-dev \
        libxml2-dev \
    && apk add --no-cache \
        vips \
        imagemagick \
    && printf "\n" | pecl install vips imagick redis \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install pdo_mysql bcmath mysqli exif gd curl mbstring zip xml fileinfo \
    && docker-php-ext-enable vips imagick redis opcache \
    # Clean up to reduce layer size
    && pecl clear-cache \
    && apk del --no-cache .build-deps \
    && rm -rf /var/cache/apk/* /tmp/* /var/tmp/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Stage 2: Production stage
FROM --platform=$TARGETPLATFORM php:${PHP_VERSION}-fpm-alpine

# Copy installed extensions and their dependencies
COPY --from=builder /usr/local/etc/php /usr/local/etc/php
COPY --from=builder /usr/local/lib/php /usr/local/lib/php
COPY --from=builder /usr/local/bin /usr/local/bin
COPY --from=builder /usr/local/sbin /usr/local/sbin

# Install runtime library dependencies for PHP extensions
RUN apk update && apk add --no-cache \
        vips \
        imagemagick \
        libwebp \
        libpng \
        libjpeg-turbo \
        freetype \
        libzip \
    && rm -rf /tmp/* /var/tmp/* \
    && rm -rf /var/cache/apk/*

# Add Composer
COPY --from=builder /usr/local/bin/composer /usr/local/bin/composer
