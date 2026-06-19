ARG NODE_VERSION=16.20.2

FROM node:${NODE_VERSION}-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

FROM deps AS builder
ARG VUE_APP_API_URL=/api
ARG VUE_APP_PUBLIC_PATH=/
ENV VUE_APP_API_URL=${VUE_APP_API_URL}
ENV VUE_APP_PUBLIC_PATH=${VUE_APP_PUBLIC_PATH}
COPY . .
RUN npm run build

FROM deps AS development
WORKDIR /app
COPY . .
EXPOSE 8080
CMD ["npm", "run", "serve", "--", "--host", "0.0.0.0", "--port", "8080"]

FROM nginxinc/nginx-unprivileged:1.27-alpine AS runtime
COPY nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget -q -O /dev/null http://127.0.0.1:8080/health || exit 1
