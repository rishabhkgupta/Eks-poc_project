# stage 1: Build
FROM golang:1.22-alpine AS builder

WORKDIR /usr/src/app/

RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o product-catalog .


# Stage 2: Run
FROM alpine:latest AS release

RUN adduser -D appuser
USER appuser

WORKDIR /usr/src/app/

COPY ./products/ ./products/
COPY --from=builder  /usr/src/app/product-catalog ./

ENV PRODUCT_CATALOG_PORT=8088
EXPOSE 8088

ENTRYPOINT [ "./product-catalog" ]