# eck-quickstart
Elastic Cloud on Kubernetes quickstart deployment (for dev, not production). Instructions for deployment can be found in [Elastic's documentation](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html). The only additional piece added to it is a [LoadBalancer service](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer) to make it externally available. If you also need to reach Elasticsearch from an external service, consider modifying the `elasticsearch-single-node.yml` file's spec.

For production, it is recommended to leverage [Elastic's helm charts](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-install-helm.html#k8s-install-helm), along with an external network load balancer, like [MetalLB](https://metallb.universe.tf), and an internal ingress controller, like [NGINX ingress controller](https://kubernetes.github.io/ingress-nginx/deploy/#cloud-deployments) as suggested [here](https://kubernetes.github.io/ingress-nginx/deploy/baremetal/#bare-metal-considerations) for bare metal deployments. For cloud deployments, consider your cloud's native load balancer, paired with an ingress controller of your choice.

# How to deploy
1. Install the Elastic Cloud on Kubernetes operator
```bash
kubectl create -f https://download.elastic.co/downloads/eck/2.6.1/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/2.6.1/operator.yaml
```

2. Deploy Elasticseach
```bash
kubectl apply -f ./elasticsearch-single-node.yml
```

3. Deploy a persistent volume for Elasticsearch
```bash
kubectl apply -f ./elasticsearch-pv.yml
```

4. Deploy a Kibana instance
```bash
kubectl apply -f ./kibana-single-node.yml
```

# How to reach kubernetes
1. Identify the node running kubernetes
```bash
kubectl get pods -o wide
```

2. Connect to the node where Kibana lives on the port specified by the quickstart-kb-http service (Example: https://\<node\_ip\>:\<service\_port\>)
```bash
kubectl get services quickstart-kb-http
```
