# kubernetes gateway api with k0s and nginx fabric

for some reason gw api does not work

## configuration

follow these steps <https://docs.nginx.com/nginx-gateway-fabric/installation/installing-ngf/manifests/>

expose with NodePort <https://docs.nginx.com/nginx-gateway-fabric/installation/expose-nginx-gateway-fabric/>

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

502 bad gateway
