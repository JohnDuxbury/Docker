# Version: 0.1.0
FROM ubuntu:14.04
MAINTAINER John Duxbury "JohnDuxbury@users.noreply.github.com"
ENV REFRESHED_AT 2015-02-01
RUN apt-get update
RUN apt-get install -y nginx
RUN echo 'Hi, I am in your container' \
    >/usr/share/nginx/html/index.html
EXPOSE 80
