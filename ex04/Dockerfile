FROM httpd:latest

RUN apt-get update && apt-get install -y vim

COPY ./public-html/ /usr/local/apache2/htdocs/