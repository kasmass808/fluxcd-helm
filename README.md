fluxcd-helm
# fluxcd configuration with helm

curl -s https://fluxcd.io/install.sh | sudo bash
flux version
minikube start
kubectl get no
kubectl create ns flux-system
flux install

flux create source git fluxcd-helm
--url=https://github.com/kasmass808/fluxcd-helm
--branch=main
--interval=10m

kubectl get gitrepo -n flux-system

kubectl apply -f apps/nginx/base/gitrepository.yaml
kubectl apply -f infra/prometheus/base/helmrepository.yaml
kubectl apply -f clusters/minikube/kustomization.yaml


# fix for oci repo
kubectl edit deploy -n flux-system helm-controller

containers:
- name: manager
  env:
  - name: HELM_EXPERIMENTAL_OCI
    value: "1"

#metrics-server
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl patch deployment metrics-server -n kube-system \
  --type=json \
  -p='[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value": "--kubelet-insecure-tls"}]'
kubectl top po

