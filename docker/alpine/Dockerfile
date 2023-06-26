FROM		alpine:latest	AS base

RUN			apk add --update \
				nginx \
			&& rm -f /var/cache/apk/*

#=#=#

FROM		base			AS packed

COPY		--chown=root:root entrypoint /run

STOPSIGNAL	SIGINT

ENTRYPOINT	[ "/run/entrypoint" ]

CMD			[ "/bin/ash" ]
