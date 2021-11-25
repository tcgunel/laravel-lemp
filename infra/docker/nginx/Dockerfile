FROM nginx:1.20-alpine
LABEL maintainer="ucan-lab <yes@u-can.pro>"
SHELL ["/bin/ash", "-oeux", "pipefail", "-c"]

ENV TZ=UTC

RUN apk update && \
  apk add --update --no-cache --virtual=.build-dependencies g++

# nginx config file
COPY ./infra/docker/nginx/*.conf /etc/nginx/conf.d/

COPY ./infra/docker/nginx/ssl/certs/*.pem /etc/ssl/certs/
COPY ./infra/docker/nginx/ssl/certs/*.crt /etc/ssl/certs/
COPY ./infra/docker/nginx/ssl/private/*.key /etc/ssl/private/

COPY ./infra/docker/nginx/ssl/snippets/*.conf /etc/nginx/snippets/

WORKDIR /work/backend
