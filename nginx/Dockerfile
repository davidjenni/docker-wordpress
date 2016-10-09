FROM nginx:1.11.4-alpine
MAINTAINER David <david@davidjenni.com>

COPY nginx.conf /etc/nginx/conf.d/default.conf

WORKDIR /var/www/html
COPY index.php ./

EXPOSE 80
