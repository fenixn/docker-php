ARG PHP_VERSION
FROM php:$PHP_VERSION-fpm

# Docker PHP Extension Installer will install the dependencies of PHP extensions
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

# Install programs
RUN apt-get update; \
    apt-get upgrade; \
    # Neovim for text editing
    apt-get install -y neovim \
    # Cron for Magento 2 task scheduling
    cron;

# Install Magento 2 required PHP extensions
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions bcmath gd intl pdo_mysql soap xsl zip;

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Replace the container's www-data user id to the env file's ids to help prevent file permission issues
ARG USER_ID
ARG GROUP_ID

RUN if [ ${USER_ID:-0} -ne 0 ] && [ ${GROUP_ID:-0} -ne 0 ]; then \
    userdel -f www-data &&\
    if getent group www-data ; then groupdel www-data; fi &&\
    groupadd -g ${GROUP_ID} www-data &&\
    useradd -l -u ${USER_ID} -g www-data www-data &&\
    install -d -m 0755 -o www-data -g www-data /home/www-data &&\
    chown --changes --no-dereference --recursive \
          --from=33:33 ${USER_ID}:${GROUP_ID} \
        /home/www-data \
        /var/www \
;fi

# Use Entrypoint to run commands to start up apps
COPY ./entrypoint.sh /
RUN chmod +x /entrypoint.sh
ENTRYPOINT /entrypoint.sh

EXPOSE 9000

USER www-data
