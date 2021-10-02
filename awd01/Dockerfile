FROM php:7.3-fpm-alpine
COPY files /tmp/
COPY src /var/www/html
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
	# install somthing
	&& apk add --update --no-cache nginx \
	&& apk add curl bash \
	&& apk add vim \
	&& apk add php7-mbstring php7-json php7-session php7-fpm php7-curl php7-posix php7-fileinfo \
	&& apk add --no-cache openssh tzdata  \ 
	# install ssh
	&& cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  \
	&& mkdir /var/run/sshd \
	&& sed -i "s/#PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config  \
	&& sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config \
	&& ssh-keygen -t dsa -P "" -f /etc/ssh/ssh_host_dsa_key  \
	&& ssh-keygen -t rsa -P "" -f /etc/ssh/ssh_host_rsa_key  \
	&& ssh-keygen -t ecdsa -P "" -f /etc/ssh/ssh_host_ecdsa_key  \
	&& ssh-keygen -t ed25519 -P "" -f /etc/ssh/ssh_host_ed25519_key  \
	# configure
	&& adduser awd -g www-data -D \
	&& echo 'awd:h3zh1' | chpasswd \
	# start web AND chown
	&& mv /tmp/docker-php-entrypoint /usr/local/bin/docker-php-entrypoint \
    && chmod +x /usr/local/bin/docker-php-entrypoint \
	&& chown -R www-data:www-data /var/www/html \
    && chmod -R 777 /var/www/html 

EXPOSE 80 22

WORKDIR /var/www/html

CMD ["/bin/bash", "-c", "/usr/local/bin/docker-php-entrypoint"]
