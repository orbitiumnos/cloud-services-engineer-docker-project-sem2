#Node

FROM node:24-alpine AS builder

WORKDIR /app

ARG VUE_APP_API_URL=/api
ENV VUE_APP_API_URL=${VUE_APP_API_URL}

COPY package.json package-lock.json ./

RUN npm ci

COPY . .
RUN NODE_OPTIONS=--openssl-legacy-provider npm run build && rm -rf node_modules

# NGINX

FROM nginxinc/nginx-unprivileged:1.30-alpine

USER root

RUN apk upgrade --no-cache

WORKDIR /usr/share/nginx/html

COPY --from=builder --chown=101:101 /app/dist ./momo-store
COPY --chown=101:101 nginx.conf /etc/nginx/conf.d/default.conf

RUN chmod -R 755 /usr/share/nginx/html/momo-store

USER 101

EXPOSE 8080

#HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
#    CMD wget -q -O /dev/null http://127.0.0.1:8080/momo-store/ || exit 1

CMD ["nginx", "-g", "daemon off;"]