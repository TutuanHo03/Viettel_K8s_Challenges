FROM nginx:latest

RUN mkdir /usr/share/nginx/html/my-website2

COPY ./my-website2/* /usr/share/nginx/html/my-website2/

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 8080
