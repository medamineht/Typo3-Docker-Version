FROM php:7.2-apache-stretch
LABEL maintainer="Med Amine HT <hadjtaieb.softtodo@gmail.com>"

# Install TYPO3
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        wget \
# Configure PHP
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
# Install required 3rd party tools
        graphicsmagick && \
# Configure extensions
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install -j$(nproc) mysqli soap gd zip opcache intl && \
    echo 'always_populate_raw_post_data = -1\nmax_execution_time = 240\nmax_input_vars = 1500\nupload_max_filesize = 32M\npost_max_size = 32M' > /usr/local/etc/php/conf.d/typo3.ini && \
# Configure Apache as needed
    a2enmod rewrite && \
    apt-get clean && \
    apt-get -y purge \
        libxml2-dev libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev && \
    rm -rf /var/lib/apt/lists/* /usr/src/*

RUN cd /var/www/html && \
    wget -O - https://get.typo3.org/9.5 | tar -xzf - && \
    ln -s typo3_src-* typo3_src && \
    ln -s typo3_src/index.php && \
    ln -s typo3_src/typo3 && \
    cp typo3/sysext/install/Resources/Private/FolderStructureTemplateFiles/root-htaccess .htaccess && \
    mkdir typo3temp && \
    mkdir typo3conf && \
    mkdir fileadmin && \
    mkdir uploads && \
    touch FIRST_INSTALL && \
    chown -R www-data. .

# Configure volumes
VOLUME /var/www/html/fileadmin
VOLUME /var/www/html/typo3conf
VOLUME /var/www/html/typo3temp
VOLUME /var/www/html/uploads
