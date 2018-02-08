FROM ubuntu:16.04
RUN apt-get update && apt-get install -y \
	apt-utils \
	net-tools \
	vim \
	curl \
	git \
	wget \
	unzip \
	nginx \
	php7.0-cli \
	php7.0-cgi \
	php7.0-fpm \
	php7.0-zip \
	php7.0-curl \
	php7.0-xml \
	php-cli \
	php-mbstring 
# File permissions workaround
RUN usermod -u 1000 www-data

#COpy config
COPY www.conf /etc/php/7.0/fpm/pool.d/www.conf
COPY nginx.conf /etc/nginx/nginx.conf
COPY upstream.conf /etc/nginx/conf.d/upstream.conf

#Turn off daemon
CMD ["nginx", "-g", "daemon off;"]	

#Install php-composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp
ENV COMPOSER_VERSION 1.6.3

RUN curl -s -f -L -o /tmp/installer.php https://raw.githubusercontent.com/composer/getcomposer.org/b107d959a5924af895807021fcef4ffec5a76aa9/web/installer \
 && php -r " \
    \$signature = '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061'; \
    \$hash = hash('SHA384', file_get_contents('/tmp/installer.php')); \
    if (!hash_equals(\$signature, \$hash)) { \
        unlink('/tmp/installer.php'); \
        echo 'Integrity check failed, installer is either corrupt or worse.' . PHP_EOL; \
        exit(1); \
    }" \
 && php /tmp/installer.php --no-ansi --install-dir=/usr/bin --filename=composer --version=${COMPOSER_VERSION} \
 && composer --ansi --version --no-interaction \
 && rm -rf /tmp/* /tmp/.htaccess
