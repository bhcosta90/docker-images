help:
	@echo "make build"
	@echo "make publish"

build:
	docker build --build-arg IMAGE=php:7.1-fpm-alpine -t bhcosta90/php:7.1.cron .
	docker build --build-arg IMAGE=php:7.2-fpm-alpine -t bhcosta90/php:7.2.cron .
	docker build --build-arg IMAGE=php:7.3-fpm-alpine -t bhcosta90/php:7.3.cron .
	docker build --build-arg IMAGE=php:7.4-fpm-alpine -t bhcosta90/php:7.4.cron .

push:
	docker push bhcosta90/php:7.1.cron
	docker push bhcosta90/php:7.2.cron
	docker push bhcosta90/php:7.3.cron
	docker push bhcosta90/php:7.4.cron
