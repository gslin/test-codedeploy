#
APPLICATION_NAME?=	test-codedeploy
AWS_PROFILE?=		default
AWS_REGION?=		us-east-1
S3_BUCKET?=		gslin-codedeploy-us-east-1-test-codedeploy

#
.DEFAULT_GOAL:=		test
.PHONY:			build clean help test

#
build: vendor
	@true

clean:
	rm -f .env
	rm -fr vendor/

composer.lock: composer.json
	composer update
	test -e composer.lock && touch composer.lock

help:
	@echo "Availabile commands:"
	@echo ""
	@echo "    make clean"
	@echo "    make deploy"
	@echo "    make deploy-force"
	@echo "    make force-deploy (alias to deploy-force)"
	@echo "    make help"
	@echo "    make test (default action)"

test: build
	cp .env.circleci .env
	rm -f public/storage
	php artisan storage:link
	vendor/bin/phpcs
	vendor/bin/phpunit

vendor: composer.lock
	composer install

-include codedeploy-makefile/codedeploy.gnumakefile
