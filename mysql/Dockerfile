FROM mysql:latest

ADD my.cnf /etc/mysql/my.cnf
RUN ln -snf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime && echo Asia/Jakarta > /etc/timezone

EXPOSE 3306
