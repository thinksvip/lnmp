ARG NGINX_VERSION=latest

FROM nginx:${NGINX_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

RUN apt-get update && apt-get -y install logrotate

COPY ./logrotate.d/nginx-docker /etc/logrotate.d/

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

CMD [ "/bin/sh","-c","CMD service cron start && nginx -g 'daemon off;'" ]

# Set upstream conf and remove the default conf
# ARG PHP_UPSTREAM_CONTAINER=unix:/sock/docker.sock
# RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}; }" > /etc/nginx/conf.d/upstream.conf \
#     && rm /etc/nginx/conf.d/default.conf

# CMD ["nginx"]

# EXPOSE 80 443
