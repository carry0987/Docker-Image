# Docker-PHP
This is a Docker image for PHP-FPM, suitable for various Web application developments.  
In addition to basic PHP support, this image provides the following extensions:  

1. ImageMagick and related libmagickwand-dev packages: These are for supporting the processing and manipulation of images, such as generating captcha codes, making thumbnails, image transcoding, etc.

2. GD library: Compiled with --with-freetype, --with-jpeg, and --with-webp to support graphic processing capabilities using Freetype, JPEG, and WebP.

3. PHP PDO, Bcmath, Mysqli extensions: These are respectively for database connection, big number computation, and MySQL related operations.

4. Other extensions: Exif, Curl, Mbstring, Zip, Xml, Fileinfo, PCNTL, and OPcache.

5. Redis extension: For scenarios that need to use the Redis key-value database.

6. VIPS extension: Provides powerful support for efficient image processing with minimal resource consumption. VIPS is suitable for applications that need to handle large images or perform a significant amount of image read, write, transformation, and processing operations.

In addition, this image includes Composer - a package management tool for PHP, which allows developers to more easily introduce various open-source PHP packages. This Dockerfile installs the necessary system packages and cleans up the apt-get cache to reduce the image size.
