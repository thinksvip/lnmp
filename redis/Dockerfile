ARG REDIS_VERSION=latest

FROM redis:${REDIS_VERSION}

ARG	TIME_ZONE

# Set Timezone

RUN ln -snf /usr/share/zoneinfo/"${TIME_ZONE}" /etc/localtime && echo "${TIME_ZONE}" > /etc/timezone