IOSDK_VER?=$(shell git rev-parse --abbrev-ref HEAD)
IMG=pagopa/iosdk-openwhisk:$(IOSDK_VER)

.PHONE: build
build: public/build/bundle.js
	$(MAKE) -C actions
	docker build -t $(IMG) .

.PHONY: push
push:
	docker push $(IMG)

.PHONY: clean
clean:
	-$(MAKE) -C actions clean
	-docker rmi -f $(IMG)

.PHONY: deploy
deploy:
	$(MAKE) -C actions deploy

.PHONY: devel
devel:
	npm install
	npm run dev

.PHONY: start
start:
	docker run -ti \
	-p 3280:3280 -p 3281:3281 \
	--rm --name openwhisk --hostname openwhisk \
	-v /var/run/docker.sock:/var/run/docker.sock \
	-e CONTAINER_EXTRA_ENV=__OW_DEBUG_PORT=8081 \
	$(IMG)

.PHONY: stop
stop:
	-docker exec openwhisk stop

public/build/bundle.js: $(wildcard src/*.svelte)
	node -v | grep v10
	npm install
	npm run build


.PHONY: undeploy
undeploy:
	$(MAKE) -C actions undeploy
