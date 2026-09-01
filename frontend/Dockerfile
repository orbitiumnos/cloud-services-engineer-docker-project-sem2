#Node

FROM node:24-alpine AS builder

WORKDIR /app

ARG VUE_APP_API_URL=/api
ENV VUE_APP_API_URL=${VUE_APP_API_URL}

COPY package.json package-lock.json ./

RUN npm ci

COPY . .
RUN NODE_OPTIONS=--openssl-legacy-provider npm run build && rm -rf node_modules
#RUN npm run build

# NGINX

FROM nginxinc/nginx-unprivileged:1.30-alpine

LABEL org.opencontainers.image.title="momo-store-frontend" \
      org.opencontainers.image.description="Vue.js frontend for momo-store served by nginx"

USER root

RUN apk upgrade --no-cache

COPY --from=builder --chown=101:101 /app/dist /usr/share/nginx/html/momo-store

COPY --chown=101:101 nginx.conf /etc/nginx/conf.d/default.conf

RUN chmod -R 755 /usr/share/nginx/html/momo-store

RUN chown -R 101:101 /usr/share/nginx/html \
    && chown -R 101:101 /var/log/nginx \
    && touch /tmp/nginx.pid \
    && chown 101:101 /tmp/nginx.pid

USER 101
EXPOSE 8080

#HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
#    CMD wget -q -O /dev/null http://127.0.0.1:8080/momo-store/ || exit 1

CMD ["nginx", "-g", "daemon off;"]