# vmikube
Vagrant definition to configure a microk8s lab

## Vagrant box
https://app.vagrantup.com/moondly/boxes/vmikube

## Start minikube using the vagrant box
```
vagrant up
vagrant ssh

microk8s.start
microk8s.status
microk8s.stop
```
https://microk8s.io/docs/

### Addons
```
microk8s.enable ingress rbac storage registry dns
```

### Kubectl
```
kubectl get po -A
```

## Building a new vagrant box
```
make
```
The box will be available in `output.box`