.PHONY: update gen doc

BINARY=xx
VERSION=$(shell git describe --abbrev=0 --tags)
BUILD=$(shell git rev-parse HEAD 2> /dev/null || echo "undefined")
BUILDTIME=$(shell date +"%Y-%m-%d_%H:%M:%S")
LDFLAGS=-ldflags "-X main.Version=$(VERSION) -X main.CommitID=$(BUILD) -X main.BuildTime=$(BUILDTIME)"

build:
	go build -o $(BINARY) $(LDFLAGS) cmd/main.go
