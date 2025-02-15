FROM nginx:alpine3.18
RUN apk update && apk upgrade
COPY index.html /usr/share/nginx/html/
CMD ["nginx", "-g", "daemon off;"]
