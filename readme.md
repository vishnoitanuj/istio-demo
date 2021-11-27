# Create cluster

```
gcloud beta container clusters create istio-demo \
    --addons=Istio --istio-config=auth=MTLS_PERMISSIVE \
    --cluster-version=1.21.5-gke.1302 \
    --machine-type=n1-standard-2 \
    --num-nodes=4
```

# Setup Gcloud CLI in local
`gcloud config set project kubernetes-332720`

`gcloud container clusters get-credentials istio-demo`

`kubectl label namespace default istio-injection=enabled`

`kubectl apply -f bookinfo.yaml`

`kubectl apply -f bookinfo-gateway.yaml`

`kubectl get svc istio-ingressgateway -n istio-system`

```
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
```

`export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT`

`echo "$GATEWAY_URL"`

`echo "http://$GATEWAY_URL/productpage"`

# Destination Rules apply
`kubectl apply -f destination-rule.yaml`
`
# Traffic Splitting

`kubectl apply -f virtual-service-all-v1.yaml`

`kubectl apply -f virtual-service-reviews-50-v3.yaml`

`kubectl apply -f virtual-service-reviews-v3.yaml`


# Request Routing

`kubectl apply -f virtual-service-all-v1.yaml`

`kubectl apply -f virtual-service-reviews-test-jason-user.yaml`



# Miscellaneous
`kubectl get virtualservices -o yaml`

`kubectl get destinationrules -o yaml`

### PPT Available on my twitter
<a href="https://twitter.com/vishnoitanuj" href="__blank">https://twitter.com/vishnoitanuj</a>

PPT: <a href="https://www.icloud.com/iclouddrive/0TbJVIM0eyOplU-Cc4AV64p5Q#Istio-demo" href="__blank">Istio Slides</a>