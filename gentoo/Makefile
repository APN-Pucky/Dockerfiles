TAG:="$(shell date +%Y%m%d%H%M%S)"
pull:
	docker pull gentoo/portage:latest
	docker pull gentoo/stage3:latest
build: pull 
	TAG=$(TAG) docker-compose build
	docker-compose build
push: build
	TAG=$(TAG) docker-compose push
	docker-compose push
