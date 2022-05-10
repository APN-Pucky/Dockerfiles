name  = apnpucky/gentoo-ebuild-ci
build:
	docker build -t $(name) .
push: build
	docker push $(name)
