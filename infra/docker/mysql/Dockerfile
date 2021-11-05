FROM mariadb:10.3.23
LABEL maintainer="ucan-lab <yes@u-can.pro>"

ENV TZ=UTC

RUN mkdir "/var/lib/mysql-files"

COPY ./infra/docker/mysql/my.cnf /etc/my.cnf
