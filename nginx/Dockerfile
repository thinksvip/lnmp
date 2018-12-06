ARG NGINX_VERSION=latest

FROM nginx:${NGINX_VERSION}

ARG	TIME_ZONE=Asia/Shanghai

# Set Timezone

RUN ln -snf /usr/share/zoneinfo/"${TIME_ZONE}" /etc/localtime && echo "${TIME_ZONE}" > /etc/timezone

# Set upstream conf and remove the default conf
# ARG PHP_UPSTREAM_CONTAINER=unix:/sock/docker.sock
# RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}; }" > /etc/nginx/conf.d/upstream.conf \
#     && rm /etc/nginx/conf.d/default.conf

# CMD ["nginx"]

# EXPOSE 80 443
