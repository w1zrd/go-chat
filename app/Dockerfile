FROM golang:alpine AS builder
WORKDIR /go/src/app
COPY ["main.go", "./"]
RUN apk add upx
RUN go get

# build and compress
ENV CGO_ENABLED=0 GOOS=linux
RUN go build -a -v -ldflags "-s -w" -o main . && upx -9 -k main

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /app
COPY --from=builder ["/go/src/app", "./"]

# Export necessary port
EXPOSE 80
# Command to run when starting the container
ENTRYPOINT ["./main"]