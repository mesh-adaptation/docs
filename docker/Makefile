all: build

PACKAGE		?=
OPTIONS		?=
NAMESPACE	= ghcr.io/mesh-adaptation
IMAGE		= $(PACKAGE)
TAG			= latest
USERNAME	?=
TOKEN		?=

build:
	docker build -f Dockerfile.$(PACKAGE) -t $(NAMESPACE)/$(IMAGE):$(TAG) . $(OPTIONS)

pull:
	docker pull $(NAMESPACE)/$(IMAGE):$(TAG)

login:
	docker login ghcr.io -u $(USERNAME) -p $(TOKEN)

push:
	docker push $(NAMESPACE)/$(IMAGE):$(TAG)

run:
	docker run --rm -it -v ${HOME}:${HOME} $(NAMESPACE)/$(IMAGE):$(TAG)
