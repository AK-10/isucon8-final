DIR = $(shell pwd)
MAC_HOST_IP := $(shell ifconfig en0 | grep "inet " | awk -v 'RS=" "' '{print $$2}')
LINUX_HOST_IP := $(shell ip addr show en0 | grep "inet " | awk -v 'RS=" "' '{print $$2}' | sed 's/\/.*//g' )

all: build

.PHONY: clean
clean:
	rm -rf isucoin

init:
	mkdir -p ${DIR}/bin
	curl https://raw.githubusercontent.com/golang/dep/master/install.sh | GOPATH=${DIR} DEP_RELEASE_TAG=v0.5.0 sh

deps:
	cd ${DIR}/src/bench; GOPATH=${DIR} ${DIR}/bin/dep ensure

.PHONY: build
build:
	GOPATH=${DIR} go build -v -o bin/bench bench/cmd/bench

build-isucointest:
	GOPATH=${DIR} go build -v -o bin/isucointest bench/cmd/isucointest


# mac用
# ifconfigと同等のコマンドを使えばlinuxでもできる

# $ ifconfig en0
# en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
# 	options=400<CHANNEL_IO>
# 	ether 78:4f:43:6c:ee:e0
# 	inet6 fe80::8ed:2fb8:4ae9:8169%en0 prefixlen 64 secured scopeid 0x5
# 	inet 192.168.11.1 netmask 0xffffff00 broadcast 192.168.11.255
# 	nd6 options=201<PERFORMNUD,DAD>
# 	media: autoselect
# 	status: active
mac_start:
	./bin/bench \
        -appep=http://localhost \
        -bankep=http://${MAC_HOST_IP}:5515 \
        -logep=http://${MAC_HOST_IP}:5516 \
        -internalbank=http://${MAC_HOST_IP}:5515 \
        -internallog=http://${MAC_HOST_IP}:5516

linux_start:
	./bin/bench \
        -appep=http://localhost \
        -bankep=http://${LINUX_HOST_IP}:5515 \
        -logep=http://${LINUX_HOST_IP}:5516 \
        -internalbank=http://${LINUX_HOST_IP}:5515 \
        -internallog=http://${LINUX_HOST_IP}:5516

