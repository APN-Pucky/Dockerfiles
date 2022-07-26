TAG:="$(shell date +%Y%m%d%H%M%S)"
build:
	TAG=$(TAG) docker-compose build
	docker-compose build
push: build
	TAG=$(TAG) docker-compose push
	docker-compose push