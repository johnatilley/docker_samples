FROM ubuntu:22.04

# Set up timezone so that installations like PHP do not ask about it
ENV TZ="Europe/London"
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Add the repository for previous php versions
RUN apt update && apt install -y software-properties-common && rm -rf /var/lib/apt/lists/*
RUN add-apt-repository ppa:ondrej/php

# Install Apache and PHP 7.4 (change this to your needs)
RUN apt update && apt install -y apache2 wget php7.4 libapache2-mod-php7.4

# Install php extensions
RUN apt update && apt install -y php7.4-mysql php7.4-curl php7.4-gd php7.4-mcrypt php7.4-zip

## Apache setup
COPY docker/http/apache2/mods-available/dir.conf /etc/apache2/mods-available/dir.conf

# Set www-data to have UID 1000
RUN usermod -u 1000 www-data;

# ### install composer
# RUN cd ~; php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"; \
#     php composer-setup.php --install-dir=/usr/local/bin --filename=composer;

WORKDIR /var/www/html

# Install Zsh to make life easier using https://github.com/deluan/zsh-in-docker
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.3/zsh-in-docker.sh)" -- -t evan
COPY docker/http/.zshrc* /root/
VOLUME [ "/tmp/zsh" ]

CMD ["apachectl", "-D", "FOREGROUND"]