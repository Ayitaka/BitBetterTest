FROM alpine:latest

RUN apk update && \
  apk add --no-cache openssl && \
  rm -rf "/var/cache/apk/*"

COPY ./.keys/docker-generate-keys.sh /script/

WORKDIR /certs
VOLUME /certs

CMD [ "/bin/sh", "/script/docker-generate-keys.sh"]
