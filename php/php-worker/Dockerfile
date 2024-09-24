ARG PHP_WORKER_VERSION=php-apm

FROM thinksvip/lnmp:${PHP_WORKER_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

RUN apk update \
	&& apk upgrade \
	&& apk add supervisor \
	&& rm -rf /var/cache/apk/*


WORKDIR /etc/supervisor/conf.d/