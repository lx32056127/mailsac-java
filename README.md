# mailsac-java
A mailsac client written by Java


# 构建
docker build mailsac .

#启动前工作

killall sendmail
/etc/init.d/postfix stop
chkconfig --level 2345 postfix off
chkconfig --level 2345 sendmail off

# 启动
docker run -d -p 25:25 -p 587:587 -p 3000:3000 mailsac

