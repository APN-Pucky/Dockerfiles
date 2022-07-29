TAG:="$(shell date +%Y%m%d%H%M%S)"
build:
	# We run this twice so that latest is slightly older, but also existent for dependent builds
	docker-compose build
	TAG=$(TAG) docker-compose build
	docker-compose build
push: build
	TAG=$(TAG) docker-compose push
	docker-compose push