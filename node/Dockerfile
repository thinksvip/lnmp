ARG NODE_VERSION=alpine

FROM node:${NODE_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

# Set Timezone
# https://wiki.alpinelinux.org/wiki/Setting_the_timezone
ARG TIME_ZONE=Asia/Shanghai
RUN apk --no-cache add tzdata && \
    cp "/usr/share/zoneinfo/${TIME_ZONE}" /etc/localtime && \
    echo "${TIME_ZONE}" > /etc/timezone && \
    apk del tzdata

# Create app directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# Install app dependencies
COPY package.json /usr/src/app/

RUN apk --no-cach add --update \
    python3 \
    py-pip \
    build-base

RUN npm install

# Bundle app source
COPY ./laravel-echo-server/laravel-echo-server.json /usr/src/app/laravel-echo-server.json

EXPOSE 3000
CMD [ "npm", "start", "--force" ]
