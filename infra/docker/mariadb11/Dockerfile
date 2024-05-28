FROM mariadb:11.3.2
LABEL maintainer="ucan-lab <yes@u-can.pro>"

ENV TZ=UTC

RUN apt-get update && \
    apt-get install -y tzdata && \
    mkdir -p /var/lib/mysql-files /var/log/mysql && \
    chown -R mysql:mysql /var/lib/mysql-files /var/log/mysql

COPY ./infra/docker/mariadb11/my.cnf /etc/my.cnf
