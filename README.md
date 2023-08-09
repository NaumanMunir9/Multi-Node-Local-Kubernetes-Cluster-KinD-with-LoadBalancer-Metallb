# Create Multi-Node Local Kubernetes Cluster (KinD) with LoadBalancer (Metallb)

## Create Kubernetes Cluster with KinD

[KinD Documentation](https://kind.sigs.k8s.io/)

- kind is a tool for running local Kubernetes clusters using Docker container “nodes”.
- kind was primarily designed for testing Kubernetes itself, but may be used for local development or CI.

#### KinD config to create multi-node cluster

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

#### Installing Metallb in KinD Kubernetes

[Installing Metallb in KinD](https://kind.sigs.k8s.io/docs/user/loadbalancer/)

##### Apply MetalLB manifest

```shell
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.7/config/manifests/metallb-native.yaml
```

```shell
kubectl wait --namespace metallb-system \
                --for=condition=ready pod \
                --selector=app=metallb \
                --timeout=90s
```

##### Setup address pool used by loadbalancers

```shell
docker network inspect -f '{{.IPAM.Config}}' kind
```

```shell
kubectl apply -f https://kind.sigs.k8s.io/examples/loadbalancer/metallb-config.yaml
```
