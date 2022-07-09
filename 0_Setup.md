# 0. Setup Required Software Environment

## 1. Build minimal CentOS 8 Stream VM in Virtualbox

## 2. Install Docker

## 3. Install kubectl

  https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

```
  curl -LO "https://dl.k8s.io/release/$(curl -L -s \
  https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  kubectl version --client
```

## 4. Install minikube

  https://minikube.sigs.k8s.io/docs/start/

```
  cd ~/Downloads
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

