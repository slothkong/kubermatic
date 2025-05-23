# Copyright 2020 The Kubermatic Kubernetes Platform contributors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

export CGO_ENABLED=0
export GOFLAGS ?= -trimpath

DOCKER_REPO ?= "quay.io/kubermatic"

.PHONY: lb-updater
lb-updater:
	go build -o ./_build/lb-updater -v k8c.io/kubermatic/v2/cmd/nodeport-proxy/lb-updater

.PHONY: envoy-manager
envoy-manager:
	go build -o ./_build/envoy-manager -v k8c.io/kubermatic/v2/cmd/nodeport-proxy/envoy-manager

.PHONY: build
build: envoy-manager lb-updater

.PHONY: clean
clean:
	rm -f envoy-manager lb-updater

.PHONY: docker
docker: build
	docker build --tag $(DOCKER_REPO)/nodeport-proxy:$(TAG) --label "org.opencontainers.image.version=$(TAG)" .

.PHONY: test
test:
	go test ./...
