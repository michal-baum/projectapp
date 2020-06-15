FROM node:14.4.0-buster-slim as build

ARG NODE_ENV=production
ENV NODE_ENV $NODE_ENV

ARG REACT_APP_SERVICE_HOST=http://localhost:8080
ENV REACT_APP_SERVICE_HOST $REACT_APP_SERVICE_HOST

WORKDIR /code

COPY package.json package-lock.json ./

RUN npm ci

COPY . /code
RUN npm run build

FROM nginx:1.19.0

COPY --from=build /code/build /usr/share/nginx/html

RUN useradd -u 5000 app \
    && mkdir -p /var/run/nginx /var/tmp/nginx \
    && chown -R app:app /usr/share/nginx /var/run/nginx /var/tmp/nginx

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./default.conf /etc/nginx/conf.d/

USER app:app

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]