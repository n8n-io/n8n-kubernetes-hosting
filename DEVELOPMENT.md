## Setup a local kubernetes cluster
```
cat <<EOF | kind create cluster --name local --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
EOF
```

## Deploy nginx ingress controller
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Create n8n namespace
```
kubectl create namespace n8n
```

## Deploy postgres (optionally)
```
kubectl apply -f postgres.yaml
```

## Deploy n8n
```
kubectl apply -f n8n.yaml
```
