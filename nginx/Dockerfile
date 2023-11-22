ARG NGINX_VERSION=alpine 

FROM nginx:${NGINX_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

# Set Timezone
# https://wiki.alpinelinux.org/wiki/Setting_the_timezone
ARG TIME_ZONE=Asia/Shanghai
RUN apk --no-cache add tzdata && \
    cp "/usr/share/zoneinfo/${TIME_ZONE}" /etc/localtime && \
    echo "${TIME_ZONE}" > /etc/timezone && \
    apk del tzdata

RUN apk update \
    && apk upgrade \
    && apk --update add logrotate

RUN set -x ; \
    addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data && exit 0 ; exit 1
    
# Create 'messages' file used from 'logrotate'
RUN touch /var/log/messages

COPY ./logrotate.d/nginx /etc/logrotate.d/

# Clean up
RUN rm -rf /tmp/* /var/cache/apk/*

CMD crond -l 2 -b && nginx -g 'daemon off;'

# Set upstream conf and remove the default conf
# ARG PHP_UPSTREAM_CONTAINER=unix:/sock/docker.sock
# RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}; }" > /etc/nginx/conf.d/upstream.conf \
#     && rm /etc/nginx/conf.d/default.conf

# CMD ["nginx"]

# EXPOSE 80 443
