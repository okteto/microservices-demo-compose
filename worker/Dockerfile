FROM golang:1.22-bullseye as dev

WORKDIR /app

ADD go.mod go.sum ./

ADD . .
RUN go mod tidy
RUN go mod download all

RUN CGO_ENABLED=0 GOOS=linux go build -v -o /usr/local/bin/worker main.go

FROM scratch

COPY --from=dev /usr/local/bin/worker /usr/local/bin/worker

ENTRYPOINT ["/usr/local/bin/worker"]
