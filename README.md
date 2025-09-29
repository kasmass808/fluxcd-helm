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
