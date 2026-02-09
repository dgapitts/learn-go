# Basic Go Makefile

BINARY_NAME=hello_world

.PHONY: build run clean test

build:
	go build -o ${BINARY_NAME} hello.go

run: build
	./${BINARY_NAME}

test:
	go test ./...

clean:
	go clean
	rm -f ${BINARY_NAME}
