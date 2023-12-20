# kubernetes gateway api with k0s and nginx fabric

for some reason gw api does not work

## configuration

### k8s runtime installation

[install k0s](https://docs.k0sproject.io/v1.28.4+k0s.0/)

```bash
curl -sSLf https://get.k0s.sh | sudo sh
```

create a cluster

```bash
sudo k0s install controller --single
```

configure user to use kubectl

```bash
mkdir ~/.kube
touch ~/.kube/config
sudo k0s kubeconfig admin > ~/.kube/config
```

or create a kind cluster with the following command:

```bash
 kind create cluster --config create-kind-cluster.yml
```

### optional - [install k9s](https://github.com/derailed/k9s?tab=readme-ov-file#installation)

```bash
# NOTE: The dev version will be in effect!
go install github.com/derailed/k9s@latest
```

### configure cluster to run Gateway API

follow these steps <https://docs.nginx.com/nginx-gateway-fabric/installation/installing-ngf/manifests/>

```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.0.0/standard-install.yaml
kubectl apply -f https://github.com/nginxinc/nginx-gateway-fabric/releases/download/v1.1.0/crds.yaml
kubectl apply -f https://github.com/nginxinc/nginx-gateway-fabric/releases/download/v1.1.0/nginx-gateway.yaml
```

expose with NodePort <https://docs.nginx.com/nginx-gateway-fabric/installation/expose-nginx-gateway-fabric/>

```bash
kubectl apply -f https://raw.githubusercontent.com/nginxinc/nginx-gateway-fabric/v1.0.0/deploy/manifests/service/nodeport.yaml
```

apply the gateway manifest:

```bash
kubectl apply -f simple-gateway.yml
```

apply the HTTPRoute manifest:

```bash
kubectl apply -f simple-http-route.yml
```

apply the Service manifest:

```bash
kubectl apply -f simple-service.yml
```

apply the deployment manifest:

```bash
kubectl apply -f simple-deployment.yml
```

## expected result

hello world from _clusterip:80_ or _localhost:exposed-nodeport_

## actual

502 bad gateway for k0s

connection resets with kind

## test

```bash
curl -i http://localhost:30255
curl -i http://10.104.3.237
```
