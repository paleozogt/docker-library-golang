PREFIX=
IMAGE_NAME=golang
IMAGE_VER=1.16.15-sid
BUILD_OPTS=

ARCHES=amd64 i386 arm64v8 arm32v7 ppc64le ppc64
IMAGES=$(addprefix $(PREFIX)/$(IMAGE_NAME):$(IMAGE_VER)-, $(ARCHES))

BUILD_TARGETS=$(addprefix image-, $(ARCHES))
images: $(BUILD_TARGETS)
$(BUILD_TARGETS): Dockerfile
		$(eval ARCH = $(word 2, $(subst -, ,$@)))
		docker build $(BUILD_OPTS) --build-arg ARCH=$(ARCH) --build-arg PREFIX=$(PREFIX) -t $(PREFIX)/$(IMAGE_NAME):$(IMAGE_VER)-$(ARCH) .

CLEAN_TARGETS=$(addprefix clean-, $(ARCHES))
clean: $(CLEAN_TARGETS)
$(CLEAN_TARGETS):
		$(eval ARCH = $(word 2, $(subst -, ,$@)))
		docker rmi $(PREFIX)/$(IMAGE_NAME):$(IMAGE_VER)-$(ARCH)

PUBLISH_TARGETS=$(addprefix publish-, $(ARCHES))
publish: $(PUBLISH_TARGETS)
		docker manifest create $(PREFIX)/$(IMAGE_NAME):$(IMAGE_VER) $(IMAGES)
		docker manifest push $(PREFIX)/$(IMAGE_NAME):$(IMAGE_VER)
$(PUBLISH_TARGETS):
		$(eval ARCH = $(word 2, $(subst -, ,$@)))
		docker push $(PREFIX)/$(IMAGE_NAME):$(IMAGE_VER)-$(ARCH)
