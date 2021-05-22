FROM nginx:1.20-alpine
ARG APP_NAME
ARG APP_ROOT

RUN apk update && apk add vim openssl openssl-dev sed

ENV COUNTRY=JP
ENV STATE=Saitama
ENV ORGANIZATION=oreore
ENV DOMAIN=localhost

WORKDIR /etc/nginx/conf.d/ssl
RUN echo 'subjectAltName = DNS:your-domain.com, IP:127.0.0.1' > SAN.txt \
    && openssl genrsa -out server.key 2048 \
    && openssl req -new -key server.key -out server.csr -subj "/C=$COUNTRY/ST=$STATE/O=$ORGANIZATION/CN=$DOMAIN" \
    && openssl x509 -req -in server.csr -days 3650 -sha256 -extfile SAN.txt -signkey server.key -out server.crt 


COPY ./files/laravel-vhost.conf /etc/nginx/conf.d/default.conf
WORKDIR /etc/nginx/conf.d
RUN sed -i.bk "s@/var/www/lfreeze@/var/www/$APP_NAME@" /etc/nginx/conf.d/default.conf

