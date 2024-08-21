FROM golang:latest

RUN mkdir /backend

WORKDIR /backend

RUN go get https://github.com/PaoGRodrigues/tfi-backend
COPY go.mod go.sum ./
RUN go.mod dowload

RUN go build -o /backend/main /app/main.go
EXPOSE 8080

ENV GIN_MODE=release

CMD ["/backend/main"]