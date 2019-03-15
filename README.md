#### Alpine Linux 3.8/3.9 wkhtmltopdf 0.12.5 (with patched qt)

Based on [alloylab/Docker-Alpine-wkhtmltopdf](https://github.com/alloylab/Docker-Alpine-wkhtmltopdf)

Alpine has wkhtmltopdf package but with unpatched qt, therefor not all wkhtmltopdf features can be used. 
This container builds wkhtmltopdf and patched qt.

Build step (Alpine 3.9):

```sh
# build the binary
docker build -t docker-alpine-wkhtmltopdf-patched-qt .
```

Usage (Alpine 3.9):

```Dockerfile
FROM docker-alpine-wkhtmltopdf-patched-qt:latest as wkhtmltopdf
FROM alpine:3.9

RUN apk --update --no-cache add \
    libgcc \
    libstdc++ \
    musl \
    qt5-qtbase \
    qt5-qtbase-x11 \
    qt5-qtsvg \
    qt5-qtwebkit \
    ttf-freefont \
    ttf-dejavu \
    ttf-droid \
    ttf-liberation \
    ttf-ubuntu-font-family \
    fontconfig

# Add openssl dependencies for wkhtmltopdf
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.8/main' >> /etc/apk/repositories && \
    apk add --no-cache libcrypto1.0 libssl1.0

# Add wkhtmltopdf
COPY --from=wkhtmltopdf /bin/wkhtmltopdf /bin/wkhtmltopdf
```
