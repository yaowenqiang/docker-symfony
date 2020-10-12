# See https://github.com/docker-library/php/blob/master/7.1/fpm/Dockerfile
FROM php:7.1-fpm
ARG TIMEZONE

MAINTAINER Maxence POUTORD <maxence.poutord@gmail.com>

RUN echo "deb http://mirrors.aliyun.com/debian/ buster main non-free contrib" > /etc/apt/sources.list \
&& echo "deb-src http://mirrors.aliyun.com/debian/ buster main non-free contrib" >> /etc/apt/sources.list \
&& echo "deb http://mirrors.aliyun.com/debian-security buster/updates main" >> /etc/apt/sources.list \
&& echo "deb-src http://mirrors.aliyun.com/debian-security buster/updates main" >> /etc/apt/sources.list \
&& echo "deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib" >> /etc/apt/sources.list \
&& echo "deb-src http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib" >> /etc/apt/sources.list \
&& echo "deb http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib" >> /etc/apt/sources.list \
&& echo "deb-src http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib" >> /etc/apt/sources.list \
&& apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    zlib1g-dev \ 
    libmemcached-dev \
    tmux \
    procps \
    iproute2 \
    lsof \
    net-tools \
    telnet \
    netcat \
    libmcrypt-dev \
    fd-find \ 
    procps \
    iputils-ping \
    gdebi-core \
    libwebp-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libxpm-dev \
    libfreetype6-dev \
    iptables \
    neovim \
    zsh \
    curl \
    dnsutils \
    dstat \
    wget \
    zlib1g-dev

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
&& composer --version

RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
&& printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
&& "date"

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql mysqli mcrypt

# install gd

RUN docker-php-ext-configure gd --with-gd --with-webp-dir --with-jpeg-dir \
    --with-png-dir --with-zlib-dir --with-xpm-dir --with-freetype-dir \
    --enable-gd-native-ttf

RUN docker-php-ext-install gd


# install xdebug

COPY extensions.txt extensions.txt
RUN cat extensions.txt | xargs -t -n 1 -P 4 pecl install
#  memcache \
# && pecl install memcached \
# && pecl install redis \
# && pecl install xdebug \
# && docker-php-ext-enable xdebug \
# RUN docker-php-ext-enable memcache \
# && docker-php-ext-enable redis \
# && docker-php-ext-enable memcached \

RUN cat extensions.txt | xargs -t  -n 1 -P 4 docker-php-ext-enable


RUN echo "error_reporting = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "display_errors = Off" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
&& echo "xdebug.remote_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini



# RUN pecl install memcache \
# && pecl install memcached \
# && pecl install redis \
# && pecl install xdebug \
# # && docker-php-ext-enable xdebug \
# && docker-php-ext-enable memcache \
# && docker-php-ext-enable redis \
# && docker-php-ext-enable memcached \
# && echo "error_reporting = 0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "display_errors = Off" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
# && echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini


RUN echo 'alias sf="php app/console"' >> ~/.bashrc \
&& echo 'alias sf3="php bin/console"' >> ~/.bashrc \
&& echo 'alias reload-fpm="kill -USR2 1"' >> ~/.bashrc

# COPY extensions.txt extensions.txt

RUN echo "set-window-option -g mode-keys vi " > /root/.tmux.conf

RUN wget https://download.docker.com/linux/static/stable/x86_64/docker-19.03.9.tgz && \
    tar -zxvf docker-19.03.9.tgz docker/docker && \
    mv docker/docker /usr/bin/ && \
    rm docker-19.03.9.tgz && rm -rf docker

WORKDIR /var/www/symfony
