FROM node:13.7-alpine3.10

RUN apk --update add \
    && apk add build-base curl git zip unzip vim \
    && apk add --update --no-cache oniguruma-dev
WORKDIR /app