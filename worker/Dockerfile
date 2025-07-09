FROM golang:1.22-bullseye as dev

COPY bashrc /root/.bashrc

WORKDIR /app

RUN go install github.com/codegangsta/gin@v0.0.0-20230218063734-2c98d96c9244 && \
    go install github.com/go-delve/delve/cmd/dlv@v1.22.1 && \
    go install golang.org/x/tools/gopls@v0.14.2 && \
    curl -sSfL https://raw.githubusercontent.com/cosmtrek/air/master/install.sh | sh -s --

ADD go.mod go.sum ./
RUN go mod download all

ADD . .
RUN go get

RUN --mount=type=cache,target=/root/.cache/go-build CGO_ENABLED=0 GOOS=linux go build -v -o /usr/local/bin/worker main.go

FROM scratch

COPY --from=dev /usr/local/bin/worker /usr/local/bin/worker

ENTRYPOINT ["/usr/local/bin/worker"]
