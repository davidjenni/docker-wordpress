FROM nginx:1.11.4-alpine
MAINTAINER David <david@davidjenni.com>

# RUN ln -sf /dev/stdout /var/log/nginx/access.log
# RUN ln -sf /dev/stderr /var/log/nginx/error.log

COPY nginx.conf /etc/nginx/conf.d/default.conf

WORKDIR /var/www/html
COPY . ./

EXPOSE 80
