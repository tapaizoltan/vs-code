# Dockerfile minta

FROM ubuntu:latest
RUN apt update
RUN apt -y install locales
RUN DEBIAN_FRONTEND=noninteractive TZ=Europe/Budapest apt -y install tzdata
#RUN apt install software-properties-common gnupg2 curl ca-certificates curl gnupg gpg nginx -y
RUN apt install software-properties-common gnupg2 curl ca-certificates curl gnupg gpg -y
RUN add-apt-repository -y ppa:ondrej/php
RUN mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
RUN echo 'deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_18.x nodistro main' | tee /etc/apt/sources.list.d/nodesource.list
RUN apt update && apt upgrade -y
RUN apt install -y nodejs
RUN npm install -g npm
#RUN apt install -y zip git mc zsh nano
RUN apt install -y zip git mc nano
RUN apt install -y php8.3 php8.3-cli php8.3-curl php8.3-mysql php8.3-xml php8.3-mbstring php8.3-imagick php8.3-gd php8.3-bcmath php8.3-intl php8.3-zip php8.3-fpm
RUN apt install -y php8.2 php8.2-cli php8.2-curl php8.2-mysql php8.2-xml php8.2-mbstring php8.2-imagick php8.2-gd php8.2-bcmath php8.2-intl php8.2-zip php8.2-fpm
RUN apt install -y php8.1 php8.1-cli php8.1-curl php8.1-mysql php8.1-xml php8.1-mbstring php8.1-imagick php8.1-gd php8.1-bcmath php8.1-intl php8.1-zip php8.1-fpm
RUN apt install -y php7.4 php7.4-cli php7.4-curl php7.4-mysql php7.4-xml php7.4-mbstring php7.4-imagick php7.4-gd php7.4-bcmath php7.4-intl php7.4-zip php7.4-fpm
RUN apt install -y php5.6 php5.6-cli php5.6-curl php5.6-mysql php5.6-xml php5.6-mbstring php5.6-imagick php5.6-gd php5.6-bcmath php5.6-intl php5.6-zip php5.6-fpm
RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer
#RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
#RUN curl -L https://cs.symfony.com/download/php-cs-fixer-v3.phar -o php-cs-fixer
#RUN chmod a+x php-cs-fixer
#RUN mv php-cs-fixer /usr/local/bin/php-cs-fixer
RUN unlink /etc/localtime
RUN ln -s /usr/share/zoneinfo/Europe/Budapest /etc/localtime
#RUN echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
#RUN echo 'alias rnx="nginx -s reload"' >> ~/.bashrc
RUN echo 'alias php83="update-alternatives --set php /usr/bin/php8.3"' >> ~/.bashrc
RUN echo 'alias php82="update-alternatives --set php /usr/bin/php8.2"' >> ~/.bashrc
RUN echo 'alias php81="update-alternatives --set php /usr/bin/php8.1"' >> ~/.bashrc
RUN echo 'alias php74="update-alternatives --set php /usr/bin/php7.4"' >> ~/.bashrc
RUN echo 'alias php56="update-alternatives --set php /usr/bin/php5.6"' >> ~/.bashrc
#RUN echo 'alias rnx="nginx -s reload"' >> ~/.zshrc
#RUN echo 'alias php82="update-alternatives --set php /usr/bin/php8.2"' >> ~/.zshrc
#RUN echo 'alias php81="update-alternatives --set php /usr/bin/php8.1"' >> ~/.zshrc
#RUN echo 'alias php74="update-alternatives --set php /usr/bin/php7.4"' >> ~/.zshrc
#RUN echo 'alias php56="update-alternatives --set php /usr/bin/php5.6"' >> ~/.zshrc
#RUN echo "[www]\nuser = root\ngroup = root\n" > /etc/php/8.2/fpm/pool.d/zzz-docker.conf
#RUN echo "[www]\nuser = root\ngroup = root\n" > /etc/php/8.1/fpm/pool.d/zzz-docker.conf
#RUN echo "[www]\nuser = root\ngroup = root\n" > /etc/php/7.4/fpm/pool.d/zzz-docker.conf
#RUN echo "[www]\nuser = root\ngroup = root\n" > /etc/php/5.6/fpm/pool.d/zzz-docker.conf
#RUN sed -i 's/--daemonize/--daemonize -R/g' /etc/init.d/php8.2-fpm
#RUN sed -i 's/--daemonize/--daemonize -R/g' /etc/init.d/php8.1-fpm
#RUN sed -i 's/--daemonize/--daemonize -R/g' /etc/init.d/php7.4-fpm
#RUN sed -i 's/--daemonize/--daemonize -R/g' /etc/init.d/php5.6-fpm
#CMD service nginx start && service php8.2-fpm start && service php8.1-fpm start && service php7.4-fpm start && service php5.6-fpm start && tail -f /dev/null
EXPOSE 8000
CMD tail -f /dev/null