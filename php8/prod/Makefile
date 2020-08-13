help:
	@echo "Comandos disponiveis:"
	@echo "make build"
	@echo "make push"

build:
	docker build --build-arg IMAGE=php:8.0.0alpha3-fpm-alpine -t bhcosta90/php:8.0 .

push:
	docker push bhcosta90/php:8.0
