
FROM golang:1.8-alpine

VOLUME /config
EXPOSE 6060 6061

ADD .   /go/src/github.com/coreos/clair/
WORKDIR /go/src/github.com/coreos/clair/

RUN apk add --no-cache git bzr rpm xz && \
    go get -v github.com/coreos/clair/ && \
    mv /go/bin/clair /clair && \
    rm -rf /go /usr/local/go

ENTRYPOINT ["/clair"]
