FROM ubuntu:22.04

# Set up timezone so that installations like PHP do not ask about it
ENV TZ="Europe/London"
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Add the repository for previous php versions
RUN apt update && apt install -y software-properties-common && rm -rf /var/lib/apt/lists/*
RUN add-apt-repository ppa:ondrej/php

RUN apt update

# Install Apache and PHP 7.4
RUN apt install -y apache2
RUN apt install -y wget
RUN apt install -y php7.4
RUN apt install -y libapache2-mod-php7.4

# Install php extensions
RUN apt install -y php7.4-bcmath
RUN apt install -y php7.4-calendar
RUN apt install -y php7.4-cli
RUN apt install -y php7.4-common
RUN apt install -y php7.4-ctype
RUN apt install -y php7.4-curl
RUN apt install -y php7.4-dom
RUN apt install -y php7.4-ftp
RUN apt install -y php7.4-gd
RUN apt install -y php7.4-iconv
RUN apt install -y php7.4-imap
RUN apt install -y php7.4-json
RUN apt install -y php7.4-mbstring
RUN apt install -y php7.4-mcrypt
RUN apt install -y php7.4-mysql
RUN apt install -y php7.4-mysqli
RUN apt install -y php7.4-mysqlnd
RUN apt install -y php7.4-pdo
RUN apt install -y php7.4-phar
RUN apt install -y php7.4-posix
RUN apt install -y php7.4-simplexml
RUN apt install -y php7.4-soap
RUN apt install -y php7.4-sockets
RUN apt install -y php7.4-sqlite3
RUN apt install -y php7.4-tokenizer
RUN apt install -y php7.4-xml
RUN apt install -y php7.4-xmlwriter
RUN apt install -y php7.4-xmlreader
RUN apt install -y php7.4-xsl
RUN apt install -y php7.4-zip

# Install Cron
RUN apt install cron nano
COPY docker/http/cron/crontab /etc/cron.d/cron
RUN crontab /etc/cron.d/cron

## Apache setup
COPY docker/http/apache2/mods-available/dir.conf /etc/apache2/mods-available/dir.conf
COPY docker/http/apache2/sites-available/localhost.conf /etc/apache2/sites-available/localhost.conf

RUN a2enmod rewrite headers expires
RUN a2dissite 000-default
RUN a2ensite localhost

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