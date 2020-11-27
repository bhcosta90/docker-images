help:
	@echo "Comandos disponiveis:"
	@echo "make build"
	@echo "make push"

build:
	docker build --build-arg IMAGE=php:7.2-fpm-alpine -t bhcosta90/php:7.2 .
	docker build --build-arg IMAGE=php:7.3-fpm-alpine -t bhcosta90/php:7.3 .
	docker build --build-arg IMAGE=php:7.4-fpm-alpine -t bhcosta90/php:7.4 .

push:
	docker push bhcosta90/php:7.1
	docker push bhcosta90/php:7.2
	docker push bhcosta90/php:7.3
	docker push bhcosta90/php:7.4