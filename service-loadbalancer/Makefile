all: push

# 0.0 shouldn't clobber any released builds
TAG ?= 0.4
PREFIX ?= lan.dhms.cloud:5000/servicelb
SRC = service_loadbalancer.go loadbalancer_log.go

rootfs/service_loadbalancer: $(SRC) Makefile
	CGO_ENABLED=0 GOOS=linux godep go build -a -installsuffix cgo -ldflags '-w' -o $@ $(SRC)

container: rootfs/service_loadbalancer
	docker build --pull -t $(PREFIX):$(TAG) rootfs

push: container
	docker push $(PREFIX):$(TAG)

clean:
	docker rmi -f $(PREFIX):$(TAG) || true
	rm -f rootfs/service_loadbalancer

.PHONY: container push
