# Docker

Configuração de docker, trocamos a timezone para impedir problemas na conversão de datas.

## Dev
```yml
# Dockerfile
FROM node:16-alpine

RUN set -x \
    && chmod 775 /usr/local/bin/* \
    && chmod +x /usr/local/bin/*.sh \
    && mkdir /server \
    && apk --no-cache add tzdata \
	  && cp /usr/share/zoneinfo/Brazil/East /etc/localtime \
    && echo Brazil/East > /etc/timezone \
	  && apk del tzdata 

WORKDIR /server

EXPOSE 5000
EXPOSE 9229

CMD yarn start:dev
```

```yml
# docker-compose.yml
version: '3'
services:

  server:
    container_name: your-container-name
    build:
      context: .
      dockerfile: docker/dev/Dockerfile # troquei o caminho
    volumes:
      - .:/server # pasta mapeado dentro do docker
    ports:
      - 5000:5000
      - 9229:9229 # porta de debug
    env_file: .env
```

Adicione as configurações abaixo para poder debuggar no VSCode:

```jsonc
//.vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [{
    "type": "node",
    "request": "attach",
    "name": "Debug",
    "address": "127.0.0.1",
    "port": 9229,
    "protocol": "inspector",
    "restart": true,
    "sourceMaps": true,
    "sourceMapPathOverrides": {
      "/server/*": "${workspaceRoot}/*"
    },
    "skipFiles": [
      "${workspaceRoot}/node_modules/**/*.js",
      "<node_internals>/**/*.js"
    ]
  }]
}
```

## Prod: 
```yml
# Dockerfile
FROM node:16-alpine

ARG VERSION

ENV VERSION=$VERSION

WORKDIR /app
COPY . /app

RUN set -x \
    && yarn \
    && yarn test \
    && yarn build

FROM node:16-alpine

ARG VERSION

ENV VERSION=$VERSION

WORKDIR /app

COPY --from=0 /app/bin /app/bin
COPY --from=0 /app/package.json /app
COPY --from=0 /app/yarn.lock /app

RUN set -x \
    && apk --no-cache add tzdata \
	  && cp /usr/share/zoneinfo/Brazil/East /etc/localtime \
    && echo Brazil/East > /etc/timezone \
	  && apk del tzdata \
    && yarn install --prod \
    && yarn cache clean

CMD node /app/bin/index.js
```