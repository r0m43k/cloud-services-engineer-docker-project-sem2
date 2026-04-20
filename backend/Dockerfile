FROM golang:1.19-alpine
WORKDIR /app
COPY . .
RUN go build -o server ./cmd/api && rm -rf /var/cache/*
EXPOSE 8081
CMD ["./server"]