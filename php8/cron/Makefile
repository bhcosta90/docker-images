help:
	@echo "make build"
	@echo "make publish"

build:
	docker build --build-arg IMAGE=php:8.0-rc-fpm-alpine -t bhcosta90/php:8.0.cron .

push:
	docker push bhcosta90/php:8.0.cron