# FROM golang:1.18
FROM golang:1.20

ENV GOPROXY https://goproxy.cn,direct
WORKDIR /go-pro/mall
COPY . .
# RUN go mod tidy && go mod verify &&
# RUN go build -o main

CMD ["go run /go-pro/mall/main.go"]
