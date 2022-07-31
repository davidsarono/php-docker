FROM mysql:latest

COPY mysqld.cnf /etc/mysql/mysql.conf.d/mysqld.cnf

RUN ln -snf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime && echo Asia/Jakarta > /etc/timezone

EXPOSE 3306
