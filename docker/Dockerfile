# Docker image for mailsac
FROM centos:6
MAINTAINER Junfeng Wu

# Update
RUN yum -y upgrade

# MongoDB
RUN echo $'[mongodb-org-3.2]\n\
name=MongoDB Repository\n\
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/\n\
gpgcheck=0\n\
enabled=1\n'\
> /etc/yum.repos.d/mongodb.repo
RUN yum -y install mongodb-org

# Redis-server
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
RUN rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-6.rpm
RUN yum -y --enablerepo=remi install redis

# Node.js
RUN curl --silent --location https://rpm.nodesource.com/setup_5.x | bash -
RUN yum -y install nodejs

# Mailsac
RUN yum -y install git
RUN git clone https://github.com/ruffrey/mailsac.git /opt/mailsac
RUN sed -i "s/definitely-change-this/`cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1`/g" /opt/mailsac/config/default.json
RUN sed -i "s/change-this/`cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1`/g" /opt/mailsac/config/default.json
RUN npm install /opt/mailsac

# Entry point
RUN echo $'/var/log/mailsac.log {\n\
    weekly\n\
    rotate 26\n\
    size 10M\n\
    create\n\
    su root\n\
    compress\n\
    delaycompress\n\
}\n\
' > /etc/logrotate.d/mailsac
ENTRYPOINT service mongod start && service redis start && cd /opt/mailsac && npm start >> /var/log/mailsac.log 2>&1  && tail -F /var/log/mailsac.log
EXPOSE 25
EXPOSE 587
EXPOSE 3000
