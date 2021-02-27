.PHONY: update gen doc

BINARY=teg-devops-openapi
VERSION=$(shell git describe --abbrev=0 --tags)
BUILD=$(shell git rev-parse HEAD 2> /dev/null || echo "undefined")
BUILDTIME=$(shell date +"%Y-%m-%d_%H:%M:%S")
LDFLAGS=-ldflags "-X main.Version=$(VERSION) -X main.CommitID=$(BUILD) -X main.BuildTime=$(BUILDTIME)"

build:
	go build -o $(BINARY) $(LDFLAGS) cmd/main.go

update:
	protoc --go_out=.. api/common/common.proto
	trpc create --protofile=api/openapi.proto --rpconly --alias --protocol=http -o=api
	cd api && make

gen:
	generate-trpc-service-code -mode=append -source=api/openapi.proto -target=service/server/server.go

doc:
	docker run --name swagger --rm -p 81:8080 -e SWAGGER_JSON=/data/apidocs.swagger.json -v `pwd`:/data swaggerapi/swagger-ui 

dev:
	go run cmd/main.go -conf=./trpc_go_local.yaml

dev2:
	go run cmd/main.go -conf=./trpc_go_dev2.yaml

mock:
	rm -rf ./test/mocks && mockery --all --keeptree --output=./test/mocks	

upgrade:
	rm go.mod go.sum
	go clean --modcache
	go mod init openapi
	go mod tidy
