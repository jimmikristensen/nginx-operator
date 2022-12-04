# nginx-operator
// TODO(user): Add simple overview of use/purpose

## Scaffolding
```
# Init operator project
operator-sdk init --plugins go/v4-alpha --domain nginx.example.com --repo github.com/jimmikristensen/nginx-operator --skip-go-version-check -- project-name nginx-operator

# scaffold API and controller
operator-sdk create api --group webserver --version v1alpha1 --kind Nginx --plugins="deploy-image/v1-alpha" --image=nginx:alpine

# update Makefile to name the operator docker image by name and version
-IMG ?= controller:latest
+IMG ?= $(IMAGE_TAG_BASE):$(VERSION)

# build the operator image
make docker-build

# load the operator docker image onto kind
kind load docker-image nginx.example.com/nginx-operator:0.0.1

# to list images available in rge kind cluster
docker exec -it $(kind get clusters | head -1)-control-plane crictl images

# deploy the operator
make deploy

# after deployment you should see the controller and the crd in the cluster
# look for pod(s) running the controller and CRDs in the cluster

# Deploy the CR
kubectl apply -f config/samples/webserver_v1alpha1_nginx.yaml
```

## Description
// TODO(user): An in-depth paragraph about your project and overview of use

## Getting Started
You’ll need a Kubernetes cluster to run against. You can use [KIND](https://sigs.k8s.io/kind) to get a local cluster for testing, or run against a remote cluster.
**Note:** Your controller will automatically use the current context in your kubeconfig file (i.e. whatever cluster `kubectl cluster-info` shows).

### Running on the cluster
1. Install Instances of Custom Resources:

```sh
kubectl apply -f config/samples/
```

2. Build and push your image to the location specified by `IMG`:
	
```sh
make docker-build docker-push IMG=<some-registry>/nginx-operator:tag
```
	
3. Deploy the controller to the cluster with the image specified by `IMG`:

```sh
make deploy IMG=<some-registry>/nginx-operator:tag
```

### Uninstall CRDs
To delete the CRDs from the cluster:

```sh
make uninstall
```

### Undeploy controller
UnDeploy the controller to the cluster:

```sh
make undeploy
```

## Contributing
// TODO(user): Add detailed information on how you would like others to contribute to this project

### How it works
This project aims to follow the Kubernetes [Operator pattern](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)

It uses [Controllers](https://kubernetes.io/docs/concepts/architecture/controller/) 
which provides a reconcile function responsible for synchronizing resources untile the desired state is reached on the cluster 

### Test It Out
1. Install the CRDs into the cluster:

```sh
make install
```

2. Run your controller (this will run in the foreground, so switch to a new terminal if you want to leave it running):

```sh
make run
```

**NOTE:** You can also run this in one step by running: `make install run`

### Modifying the API definitions
If you are editing the API definitions, generate the manifests such as CRs or CRDs using:

```sh
make manifests
```

**NOTE:** Run `make --help` for more information on all potential `make` targets

More information can be found via the [Kubebuilder Documentation](https://book.kubebuilder.io/introduction.html)

## License

Copyright 2022.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

