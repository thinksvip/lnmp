ARG MYSQL_VERSION=5.7

FROM mysql:${MYSQL_VERSION}

ARG	TIME_ZONE=Asia/Shanghai

# Set Timezone  # give access
USER root
RUN ln -snf /usr/share/zoneinfo/"${TIME_ZONE}" /etc/localtime && echo "${TIME_ZONE}" > /etc/timezone && chown -R mysql:mysql /var/log/mysql/