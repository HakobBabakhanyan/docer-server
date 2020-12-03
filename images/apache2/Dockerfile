#ARG APACHE_VERSION="2.4.33"
ARG APACHE_VERSION=""
FROM httpd:${APACHE_VERSION:+${APACHE_VERSION}-}alpine

RUN apk update; \
    apk upgrade;

#RUN a2enmod rewrite

# Copy apache vhost file to proxy php requests to php-fpm container
#COPY ./conf/admin_generate.apache.conf /usr/local/apache2/conf/admin_generate.apache.conf
COPY ./key/server.key  /usr/local/apache2/conf/server.key
COPY ./key/server.crt  /usr/local/apache2/conf/server.crt

RUN sed -i \
        -e 's/^#\(Include .*httpd-ssl.conf\)/\1/' \
        -e 's/^#\(LoadModule .*mod_ssl.so\)/\1/' \
        -e 's/^#\(LoadModule .*mod_socache_shmcb.so\)/\1/' \
        conf/httpd.conf

RUN sed -i '/LoadModule rewrite_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
     sed -i 's#AllowOverride [Nn]one#AllowOverride All#' /usr/local/apache2/conf/httpd.conf


RUN echo "Include /usr/local/apache2/site/*" \
    >> /usr/local/apache2/conf/httpd.conf

