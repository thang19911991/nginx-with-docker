FROM    centos:centos6

MAINTAINER  thangnvbkhn@gmail.com

RUN cp -p /usr/share/zoneinfo/Japan /etc/localtime

# Install common packages
RUN     yum -y install wget curl sudo passwd git openssh-server vim

# Install mysql command line
RUN     yum -y install mysql

# Install epel common for installing remi repo git
RUN     yum -y install epel*

ADD     remi-release-6.rpm  /usr/local/src/remi-release-6.rpm
RUN     rpm -Uvh /usr/local/src/remi-release-6.rpm

COPY    nginx.repo  /etc/yum.repos.d/nginx.repo

RUN     yum -y install php php-fpm --enablerepo=remi-php56
RUN     yum -y install nginx

COPY    www.conf /etc/php-fpm.d/www.conf

RUN     rm -rf /etc/nginx/conf.d/default.conf
RUN     rm -rf /etc/nginx/conf.d/virtual.conf
COPY    virtual.conf /etc/nginx/conf.d/virtual.conf

# Make web directory
RUN     mkdir -p /srv/nginx
RUN     chmod 777 /srv/nginx

# Make folder test
RUN     mkdir -p /srv/nginx/test.apdev.local/public
RUN     echo "example docker container nginx server" > /srv/nginx/test.apdev.local/public/index.html

# start service
RUN     service nginx restart
RUN     chkconfig nginx on
RUN     service sshd start
RUN     chkconfig sshd on
RUN     service php-fpm restart
RUN     chkconfig php-fpm on

# Make user centos
RUN useradd -m centos
RUN echo "centos:centos" | chpasswd
# Permit centos user to user wheel
RUN gpasswd -a centos wheel

# Docker側でlistenするポート番号を指定
EXPOSE  80 22

# Launch nginx when launching the container
CMD ["nginx", "-g", "daemon off;"]