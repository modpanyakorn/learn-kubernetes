FROM node:24.5.0-alpine3.22 AS build
WORKDIR /app

COPY package*.json ./
RUN npm ci
COPY . .

ENV REACT_APP_LOCALHOST_KEY=chat-app-current-user
RUN npm run build

FROM nginx:alpine3.22

RUN apk add --no-cache gettext

COPY ./nginx.conf /etc/nginx/conf.d/default.conf.template
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 3000

CMD ["/bin/sh", "-c", "envsubst '$API_SERVICE_URL' < /etc/nginx/conf.d/default.conf.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"]
