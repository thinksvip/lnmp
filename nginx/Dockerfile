ARG NGINX_VERSION=latest

FROM nginx:${NGINX_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

RUN apt-get update && apt-get -y install logrotate

COPY ./logrotate.d/nginx /etc/logrotate.d/

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog
    
RUN set -x ; \
    addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data && exit 0 ; exit 1

CMD service cron start && nginx -g 'daemon off;'

# Set upstream conf and remove the default conf
# ARG PHP_UPSTREAM_CONTAINER=unix:/sock/docker.sock
# RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}; }" > /etc/nginx/conf.d/upstream.conf \
#     && rm /etc/nginx/conf.d/default.conf

# CMD ["nginx"]

# EXPOSE 80 443
