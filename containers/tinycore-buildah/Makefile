DOCKER = sudo podman

IMAGE_NAME := tinycore-buildah
IMAGE_TAG := 1.8

.PHONY: all clean build

all: build

build:
	$(DOCKER) build -t $(IMAGE_NAME):$(IMAGE_TAG) .

clean:
	$(DOCKER) images $(IMAGE_NAME):$(IMAGE_TAG) && $(DOCKER) rmi $(IMAGE_NAME):$(IMAGE_TAG) || true
