---
layout: post
title: "Cài đặt AWX on Ubuntu"
subtitle: "Khởi tạo cấu hình  cơ bản"
date: 2022-10-30 00:45:13 -0400
background: '/img/posts/01.jpg'
---
```
$ sudo apt install snapd

$ sudo snap install microk8s --classic
  [sudo] password for quangtv:
  microk8s (1.25/stable) v1.25.3 from Canonical✓ installed

$ sudo  usermod -a -G microk8s $USER

$ sudo chown -f -R $USER ~/.kube

$ reconnect

$ microk8s enable storage dns ingress
  Infer repository core for addon storage
  Infer repository core for addon dns
  Infer repository core for addon ingress
  DEPRECIATION WARNING: 'storage' is deprecated and will soon be removed. Please use 'hostpath-storage' instead.

  Infer repository core for addon hostpath-storage
  Enabling default storage class.
  WARNING: Hostpath storage is not suitable for production environments.

  [sudo] password for quangtv:
  deployment.apps/hostpath-provisioner created
  storageclass.storage.k8s.io/microk8s-hostpath created
  serviceaccount/microk8s-hostpath created
  clusterrole.rbac.authorization.k8s.io/microk8s-hostpath created
  clusterrolebinding.rbac.authorization.k8s.io/microk8s-hostpath created
  Storage will be available soon.
  Enabling DNS
  Applying manifest
  serviceaccount/coredns created
  configmap/coredns created
  deployment.apps/coredns created
  service/kube-dns created
  clusterrole.rbac.authorization.k8s.io/coredns created
  clusterrolebinding.rbac.authorization.k8s.io/coredns created
  Restarting kubelet
  DNS is enabled
  Enabling Ingress
  ingressclass.networking.k8s.io/public created
  ingressclass.networking.k8s.io/nginx created
  namespace/ingress created
  serviceaccount/nginx-ingress-microk8s-serviceaccount created
  clusterrole.rbac.authorization.k8s.io/nginx-ingress-microk8s-clusterrole created
  role.rbac.authorization.k8s.io/nginx-ingress-microk8s-role created
  clusterrolebinding.rbac.authorization.k8s.io/nginx-ingress-microk8s created
  rolebinding.rbac.authorization.k8s.io/nginx-ingress-microk8s created
  configmap/nginx-load-balancer-microk8s-conf created
  configmap/nginx-ingress-tcp-microk8s-conf created
  configmap/nginx-ingress-udp-microk8s-conf created
  daemonset.apps/nginx-ingress-microk8s-controller created
  Ingress is enabled

$ microk8s status
  microk8s is running
  high-availability: no
    datastore master nodes: 127.0.0.1:19001
    datastore standby nodes: none
  addons:
    enabled:
      dns                  # (core) CoreDNS
      ha-cluster           # (core) Configure high availability on the current node
      helm                 # (core) Helm - the package manager for Kubernetes
      helm3                # (core) Helm 3 - the package manager for Kubernetes
      hostpath-storage     # (core) Storage class; allocates storage from host directory
      ingress              # (core) Ingress controller for external access
      storage              # (core) Alias to hostpath-storage add-on, deprecated
    disabled:
      cert-manager         # (core) Cloud native certificate management
      community            # (core) The community addons repository
      dashboard            # (core) The Kubernetes dashboard
      gpu                  # (core) Automatic enablement of Nvidia CUDA
      host-access          # (core) Allow Pods connecting to Host services smoothly
      kube-ovn             # (core) An advanced network fabric for Kubernetes
      mayastor             # (core) OpenEBS MayaStor
      metallb              # (core) Loadbalancer for your Kubernetes cluster
      metrics-server       # (core) K8s Metrics Server for API access to service metrics
      observability        # (core) A lightweight observability stack for logs, traces and metrics
      prometheus           # (core) Prometheus operator for monitoring and logging
      rbac                 # (core) Role-Based Access Control for authorisation
      registry             # (core) Private image registry exposed on localhost:32000

$ microk8s enable storage dns ingress
  Infer repository core for addon storage
  Infer repository core for addon dns
  Infer repository core for addon ingress
  Addon core/storage is already enabled
  Addon core/dns is already enabled
  Addon core/ingress is already enabled

$ microk8s status
  microk8s is running
  high-availability: no
    datastore master nodes: 127.0.0.1:19001
    datastore standby nodes: none
  addons:
    enabled:
      dns                  # (core) CoreDNS
      ha-cluster           # (core) Configure high availability on the current node
      helm                 # (core) Helm - the package manager for Kubernetes
      helm3                # (core) Helm 3 - the package manager for Kubernetes
      hostpath-storage     # (core) Storage class; allocates storage from host directory
      ingress              # (core) Ingress controller for external access
      storage              # (core) Alias to hostpath-storage add-on, deprecated
    disabled:
      cert-manager         # (core) Cloud native certificate management
      community            # (core) The community addons repository
      dashboard            # (core) The Kubernetes dashboard
      gpu                  # (core) Automatic enablement of Nvidia CUDA
      host-access          # (core) Allow Pods connecting to Host services smoothly
      kube-ovn             # (core) An advanced network fabric for Kubernetes
      mayastor             # (core) OpenEBS MayaStor
      metallb              # (core) Loadbalancer for your Kubernetes cluster
      metrics-server       # (core) K8s Metrics Server for API access to service metrics
      observability        # (core) A lightweight observability stack for logs, traces and metrics
      prometheus           # (core) Prometheus operator for monitoring and logging
      rbac                 # (core) Role-Based Access Control for authorisation
      registry             # (core) Private image registry exposed on localhost:32000


$ sudo snap alias microk8s.kubectl kubectl
  error: cannot perform the following tasks:
  - Setup manual alias "kubectl" => "kubectl" for snap "microk8s" (cannot enable alias "kubectl" for "microk8s", it conflicts with the command namespace of installed snap "kubectl")

$ git clone https://github.com/ansible/awx-operator.git
  Cloning into 'awx-operator'...
  remote: Enumerating objects: 7487, done.
  remote: Counting objects: 100% (493/493), done.
  remote: Compressing objects: 100% (299/299), done.
  remote: Total 7487 (delta 215), reused 395 (delta 160), pack-reused 6994
  Receiving objects: 100% (7487/7487), 2.08 MiB | 9.31 MiB/s, done.
  Resolving deltas: 100% (4249/4249), done.

$ cd  awx-operator

$ export NAMESPACE=awx

$ sudo apt install make
  Reading package lists... Done
  Building dependency tree
  Reading state information... Done
  make is already the newest version (4.2.1-1.2).
  make set to manually installed.
  0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

$ make deploy
  namespace/awx created
  customresourcedefinition.apiextensions.k8s.io/awxbackups.awx.ansible.com created
  customresourcedefinition.apiextensions.k8s.io/awxrestores.awx.ansible.com created
  customresourcedefinition.apiextensions.k8s.io/awxs.awx.ansible.com created
  serviceaccount/awx-operator-controller-manager created
  role.rbac.authorization.k8s.io/awx-operator-awx-manager-role created
  role.rbac.authorization.k8s.io/awx-operator-leader-election-role created
  clusterrole.rbac.authorization.k8s.io/awx-operator-metrics-reader created
  clusterrole.rbac.authorization.k8s.io/awx-operator-proxy-role created
  rolebinding.rbac.authorization.k8s.io/awx-operator-awx-manager-rolebinding created
  rolebinding.rbac.authorization.k8s.io/awx-operator-leader-election-rolebinding created
  clusterrolebinding.rbac.authorization.k8s.io/awx-operator-proxy-rolebinding created
  configmap/awx-operator-awx-manager-config created
  service/awx-operator-controller-manager-metrics-service created
  deployment.apps/awx-operator-controller-manager created

$ ### Wait pod running 2/2

$ kubectl get pods -n $NAMESPACE

$ vi my-awx.yml
  ---
  apiVersion: awx.ansible.com/v1beta1
  kind: AWX
  metadata:
    name: awx-demo
  spec:
    service_type: nodeport

$ kubectl apply -f my-awx.yml -n awx

$ kubectl get pods -n $NAMESPACE

$ kubectl get services -n awx

$ vi my-awx-ingress.yml
  ---
  apiVersion: awx.ansible.com/v1beta1
  kind: AWX
  metadata:
    name: awx-demo
  spec:
    service_type: nodeport
    ingress_type: ingress
    hostname: awx.huce.edu.vn

$ kubectl apply -f my-awx-ingress.yml -n awx

### Wait pod running 4/4, 2/2, 1/1

$ kubectl get services -n awx

$ kubectl get secret awx-demo-admin-password -n awx -o jsonpath='{.data.password}' | base64 --decode
  q9qBqtzKNTPRAmGQRya2fn89MOZxg8cx%  
  
  ```
