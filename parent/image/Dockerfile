FROM docker.io/alpine:latest

RUN apk add --update darkhttpd
ENTRYPOINT ["/usr/bin/darkhttpd"]
CMD ["/var/www/localhost/htdocs", "--port", "8080"]

COPY index.html /var/www/localhost/htdocs/index.html
