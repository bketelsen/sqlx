NO_COLOR=\033[0m
OK_COLOR=\033[32;01m
ERROR_COLOR=\033[31;01m
WARN_COLOR=\033[33;01m
DEPS = $(go list -f '{{range .TestImports}}{{.}} {{end}}' ./...)

deps:
	@echo "$(OK_COLOR)==> Installing dependencies$(NO_COLOR)"
	@go get -d -v ./...
	@echo $(DEPS) | xargs -n1 go get -d

updatedeps:
	@echo "$(OK_COLOR)==> Updating all dependencies$(NO_COLOR)"
	@go get -d -v -u ./...
	@echo $(DEPS) | xargs -n1 go get -d -u

format:
	@echo "$(OK_COLOR)==> Formatting$(NO_COLOR)"
	go fmt ./...

test: deps
	@echo "$(OK_COLOR)==> Testing$(NO_COLOR)"
	go test ./...

lint:
	@echo "$(OK_COLOR)==> Linting$(NO_COLOR)"
	golint .

build:
	@echo "$(OK_COLOR)==> Building$(NO_COLOR)"
	GOOS=linux GOARCH=amd64 go build .

run:
	@echo "$(OK_COLOR)==> Building$(NO_COLOR)"
	go run main.go

dockerbuild: build
	@echo "$(OK_COLOR)==> Docker Build$(NO_COLOR)"
	docker build -t bketelsen/sqlxexec .

dockerrun: dockerbuild
	@echo "$(OK_COLOR)==> Docker Run$(NO_COLOR)"
	docker run -t -i --link `id -u -n`-empdb:mysql bketelsen/sqlxexec

all: format lint test build dockerbuild

# pretty colors found here!
# https://gist.github.com/akalyaev/9682712
