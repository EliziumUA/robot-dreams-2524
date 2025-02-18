FROM ubuntu:22.04
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y nginx
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]
