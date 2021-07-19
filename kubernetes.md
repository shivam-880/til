## Basic `minikube` cluster commands
```sh
$ minikube start --driver=docker
$ minikube stop
$ minikube status
$ minikube delete
```

## List minikube cluster ip address
```sh
$ minikube ip
```

## Enable Addons on a minikube cluster
```sh
$ minikube addons enable ingress
$ minikube addons list
```

## List minikube docker environment variables
```sh
$ minikube docker-env
export DOCKER_TLS_VERIFY=”1"
export DOCKER_HOST=”tcp://172.17.0.2:2376"
export DOCKER_CERT_PATH=”/home/iamsmkr/.minikube/certs”
export MINIKUBE_ACTIVE_DOCKERD=”minikube”
```

## Point local Docker daemon to the minikube internal Docker registry
```sh
$ eval $(minikube -p minikube docker-env)
```

With this when you publish docker images, they will get available in minikube's internal docker registry
```sh
$ sbt hello-impl/docker:publishLocal -Ddocker.username=iamsmkr -Ddocker.registry=index.docker.io
$ sbt hello-proxy-impl/docker:publishLocal -Ddocker.username=iamsmkr -Ddocker.registry=index.docker.io

$ docker images
REPOSITORY                                              TAG            IMAGE ID       CREATED         SIZE
iamsmkr/hello-proxy-impl                                1.0-SNAPSHOT   bccb68361d4a   3 minutes ago   391MB
iamsmkr/hello-proxy-impl                                latest         bccb68361d4a   3 minutes ago   391MB
iamsmkr/hello-impl                                      1.0-SNAPSHOT   fbcd7084caeb   4 minutes ago   393MB
iamsmkr/hello-impl                                      latest         fbcd7084caeb   4 minutes ago   393MB
adoptopenjdk/openjdk8                                   latest         9805d11af75f   39 hours ago    320MB
k8s.gcr.io/kube-proxy                                   v1.19.4        635b36f4d89f   8 months ago    118MB
k8s.gcr.io/kube-apiserver                               v1.19.4        b15c6247777d   8 months ago    119MB
k8s.gcr.io/kube-controller-manager                      v1.19.4        4830ab618586   8 months ago    111MB
k8s.gcr.io/kube-scheduler                               v1.19.4        14cd22f7abe7   8 months ago    45.7MB
us.gcr.io/k8s-artifacts-prod/ingress-nginx/controller   v0.40.2        4b26fa2d90ae   9 months ago    286MB
gcr.io/k8s-minikube/storage-provisioner                 v3             bad58561c4be   10 months ago   29.7MB
k8s.gcr.io/etcd                                         3.4.13-0       0369cf4303ff   10 months ago   253MB
jettech/kube-webhook-certgen                            v1.3.0         4d4f44df9f90   12 months ago   54.7MB
jettech/kube-webhook-certgen                            v1.2.2         5693ebf5622a   12 months ago   49MB
kubernetesui/dashboard                                  v2.0.3         503bc4b7440b   13 months ago   225MB
k8s.gcr.io/coredns                                      1.7.0          bfe3a36ebd25   13 months ago   45.2MB
kubernetesui/metrics-scraper                            v1.0.4         86262685d9ab   15 months ago   36.9MB
k8s.gcr.io/pause                                        3.2            80d28bedfe5d   17 months ago   683kB
```

## Launch minikube Dashboard
```sh
$ minikube dashboard
```

## Show minikube cluster info
```sh
$ kubectl cluster-info
```

## Create a secret
```sh
$ kubectl create secret generic application-secret --from-literal=secret="$(openssl rand -base64 48)"
```

## Apply deployment configs
```sh
$ kubectl apply -f deploy/k8s/common-config-map.yaml
$ kubectl apply -f deploy/k8s/rbac.yaml
$ kubectl apply -f deploy/k8s/prime-generator-deployment.yaml 
$ kubectl apply -f deploy/k8s/prime-generator-cluster-ip-service.yaml
```

## Delete deployment configs
```sh
$ kubectl delete -f kubernetes/hello-impl.yml
```

## Kubectl queries
```sh
$ kubectl get pods
$ kubectl get deployments
```
