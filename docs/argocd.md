# ArgoCD

## Prepare workstation

```bash
VERSION=$(curl -L -s https://raw.githubusercontent.com/argoproj/argo-cd/stable/VERSION)
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/download/v$VERSION/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

## Prepare k3s hoste

```bash
sudo apt-get update && sudo apt-get upgrade -y && sudo apt autoremove -y && sudo apt autoclean -y
sudo apt install qemu-guest-agent nfs-common open-iscsi -y
sudo reboot now
sudo modprobe iscsi_tcp
```

## Install k3s

```bash
curl -sfL https://get.k3s.io  | INSTALL_K3S_EXEC="--disable local-storage --disable servicelb --disable=traefik --disable-cloud-controller" sh -
sudo cat /etc/rancher/k3s/k3s.yaml
```

## Prepare cluster

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
k port-forward # consol access
k -n argocd get secrets argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d  # get init password
```
