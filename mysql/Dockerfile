ARG MYSQL_VERSION=5.7

FROM mysql:${MYSQL_VERSION}

ARG	TIME_ZONE

# Set Timezone

RUN ln -snf /usr/share/zoneinfo/"${TIME_ZONE}" /etc/localtime && echo "${TIME_ZONE}" > /etc/timezone 
# give access
RUN chown -R mysql:mysql /var/log/mysql/