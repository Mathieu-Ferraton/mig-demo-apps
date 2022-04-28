#FROM registry.access.redhat.com/ubi8/ubi-minimal
FROM --platform=$BUILDPLATFORM golang:1.17.8-alpine AS builder
ARG TARGETOS
ARG TARGETARCH
ARG TARGETVARIANT
ENV GOOS=${TARGETOS} \
    GOARCH=${TARGETARCH} \
    GOARM=${TARGETVARIANT}
RUN mkdir /build
WORKDIR /build
COPY apps/todolist-mongo-go/*.go .
COPY apps/todolist-mongo-go/go.mod .
COPY apps/todolist-mongo-go/go.sum .
RUN go mod download
RUN go mod tidy
RUN CGO_ENABLED=0 go build -v -a -installsuffix cgo -o app

FROM scratch
COPY --from=builder /build/app /app
COPY apps/todolist-mongo-go/resources/ /resources/
COPY apps/todolist-mongo-go/index.html .
EXPOSE 8000
# use entrypoint for debug
#ENTRYPOINT ["tail", "-f", "/dev/null"]
CMD ["./app"]
