ARG NGINX_VERSION=latest

FROM nginx:${NGINX_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

# Set upstream conf and remove the default conf
# ARG PHP_UPSTREAM_CONTAINER=unix:/sock/docker.sock
# RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}; }" > /etc/nginx/conf.d/upstream.conf \
#     && rm /etc/nginx/conf.d/default.conf

# CMD ["nginx"]

# EXPOSE 80 443
