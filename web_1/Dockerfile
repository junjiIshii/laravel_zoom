FROM centos:8

ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

RUN /bin/cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

RUN yum install -y epel-release && yum clean all

RUN rpm -ivh http://ftp.riken.jp/Linux/remi/enterprise/remi-release-8.rpm

RUN yum -y update && yum clean all

RUN yum -y install httpd && yum clean all

RUN yum -y install php74-php php74-php-mysqli php74-php-gd php74-php-mbstring php74-php-opcache php74-php-xml php74-php-pear php74-php-devel php74-php-pecl-imagick php74-php-pecl-imagick-devel php74-php-pecl-zip

RUN ln /usr/bin/php74 /usr/bin/php

COPY ./php.ini /etc/opt/remi/php74/php.ini

RUN chown -R apache:apache /var/www/html

RUN systemctl enable php74-php-fpm

RUN systemctl enable httpd 

COPY --from=composer:1.10 /usr/bin/composer /usr/bin/composer

#RUN cd ~ && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && php composer-setup.php && php -r "unlink('composer-setup.php');" && mv composer.phar /usr/local/bin/composer

VOLUME [ "/var/www/html" ]

RUN chown -R apache:apache /var/www/html

EXPOSE 80