---
layout: post
title:  Kind로 openstack상에 cluster 생성
date:   2022-02-08 10:01:35 +0300
image:  kind-cluster.png
tags:   Case-study
---

# Kind를 boostrap cluster로 하여 openstack상에 workload cluster 생성하기

* 참고사이트
  - https://cluster-api.sigs.k8s.io/user/quick-start.html
  - https://image-builder.sigs.k8s.io/capi/providers/openstack.html
  - https://ssup2.github.io/record/Kubernetes_%EC%84%A4%EC%B9%98_ClusterAPI_External_Cloud_Provider_Ubuntu_18.04_OpenStack/
  - https://www.jacobbaek.com/1101
<br/><br/>

* CAPI를 이용한 Openstack k8s cluster 구조
![clusterapi-openstack architecture](https://ssup2.github.io/images/theory_analysis/Kubernetes_ClusterAPI_Architecture_OpenStack/Kubernetes_ClusterAPI_Architecture_OpenStack.PNG)

## Kind 설치 및 boostrap cluster 생성
### 사전 요구사항
```bash
$ mkdir -p /data/docker
$ mkdir /etc/docker
$ cat > /etc/docker/daemon.json <<EOF
{
  "data-root": "/data/docker",
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
EOF

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
$ cat > /etc/apt/sources.list.d/docker.list <<EOF
deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable
EOF

$ apt-get update

// docker 설치
$ apt-get install -y docker-ce docker-ce-cli

// go, make, pip3, unzip 설치
$ apt-get install -y golang make python3-pip unzip

// VM manager 설치
$ apt install -y qemu-kvm libvirt-daemon-system libvirt-clients virtinst cpu-checker libguestfs-tools libosinfo-bin

// kubectl 최신버전
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
// kubectl 특정버전 받기
$ curl -LO https://dl.k8s.io/release/v1.23.0/bin/linux/amd64/kubectl
$ install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
$ kubectl version
```
<br/>

### Kind 설치
```bash
GO111MODULE="on" go get sigs.k8s.io/kind@v0.11.1

$ echo "export PATH=$PATH:/root/go/bin:/root/.local/bin" >> ~/.profile

root@boostrap1:/etc/sysctl.d# which kind
/root/go/bin/kind
```

- Kind create cluster 명령으로 옵션없이 설치하게 되면, localhost(127.0.0.1:port)로만 kubernetes에 접속할 수 있다. 만약 외부에서 접속할 필요가 있는 경우에는 아래와 같이 kind 구성파일을 설정하면 생성하면 된다.
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  role: control-plane
  extraPortMappings:
    containerPort: 6443
    hostPort: 6443
    listenAddress: "0.0.0.0"      # Listen address 추가
    protocol: tcp
  kubeadmConfigPatches:
    
    kind: ClusterConfiguration
    apiServer:
      certSANs:
          localhost
          127.0.0.1
          192.168.77.222        # kubernetes apiserver 인증서에 Host IP 추가.
```

 
```bash
$ kind create cluster --config ./kind-config.yaml --name edge
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED        STATUS        PORTS                                               NAMES
12679178177b   kindest/node:v1.21.1   "/usr/local/bin/entr…"   45 hours ago   Up 45 hours   0.0.0.0:6443->6443/tcp, 127.0.0.1:33123->6443/tcp   edge-control-plane

$ kubectl get pods -A
NAMESPACE            NAME                                         READY   STATUS    RESTARTS   AGE
kube-system          coredns-558bd4d5db-mqs84                     1/1     Running   0          27s
kube-system          coredns-558bd4d5db-s8k4g                     1/1     Running   0          27s
kube-system          etcd-kind-control-plane                      1/1     Running   0          30s
kube-system          kindnet-8vrf7                                1/1     Running   0          27s
kube-system          kube-apiserver-kind-control-plane            1/1     Running   0          30s
kube-system          kube-controller-manager-kind-control-plane   1/1     Running   0          30s
kube-system          kube-proxy-55pcm                             1/1     Running   0          27s
kube-system          kube-scheduler-kind-control-plane            1/1     Running   0          30s
local-path-storage   local-path-provisioner-547f784dff-fzfzc      1/1     Running   0          27s

$ docker exec -it 12df590d4375 bash
$ crictl images
IMAGE                                      TAG                  IMAGE ID            SIZE
docker.io/kindest/kindnetd                 v20210326-1e038dc5   6de166512aa22       54MB
docker.io/rancher/local-path-provisioner   v0.0.14              e422121c9c5f9       13.4MB
k8s.gcr.io/build-image/debian-base         v2.1.0               c7c6c86897b63       21.1MB
k8s.gcr.io/coredns/coredns                 v1.8.0               296a6d5035e2d       12.9MB
k8s.gcr.io/etcd                            3.4.13-0             0369cf4303ffd       86.7MB
k8s.gcr.io/kube-apiserver                  v1.21.1              94ffe308aeff9       127MB
k8s.gcr.io/kube-controller-manager         v1.21.1              96a295389d472       121MB
k8s.gcr.io/kube-proxy                      v1.21.1              0e124fb3c695b       133MB
k8s.gcr.io/kube-scheduler                  v1.21.1              1248d2d503d37       51.9MB
k8s.gcr.io/pause                           3.5                  ed210e3e4a5ba       301kB
```
<br/>

### clusterctl 설치
```bash
$ curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/v1.0.2/clusterctl-linux-amd64 -o clusterctl

$ chmod +x clusterctl
$ mv ./clusterctl /usr/local/bin/clusterctl
$ clusterctl version
clusterctl version: &version.Info{Major:"1", Minor:"0", GitVersion:"v1.0.2", GitCommit:"89db44e9a462028267ed49295359fe9db2a6a10a", GitTreeState:"clean", BuildDate:"2021-12-06T17:52:47Z", GoVersion:"go1.16.8", Compiler:"gc", Platform:"linux/amd64"}

$ 
```
<br/>

### clusterctl을 이용하여 openstack infra상의 management cluster component 설치
```bash
$ clusterctl init --infrastructure openstack
Fetching providers
Installing cert-manager Version="v1.5.3"
Waiting for cert-manager to be available...
Installing Provider="cluster-api" Version="v1.0.2" TargetNamespace="capi-system"
Installing Provider="bootstrap-kubeadm" Version="v1.0.2" TargetNamespace="capi-kubeadm-bootstrap-system"
Installing Provider="control-plane-kubeadm" Version="v1.0.2" TargetNamespace="capi-kubeadm-control-plane-system"
I1218 11:06:35.832969   13207 request.go:665] Waited for 1.037591028s due to client-side throttling, not priority and fairness, request: GET:https://127.0.0.1:35645/apis/controlplane.cluster.x-k8s.io/v1beta1?timeout=30s
Installing Provider="infrastructure-openstack" Version="v0.5.0" TargetNamespace="capo-system"

Your management cluster has been initialized successfully!

You can now create your first workload cluster by running the following:

  clusterctl generate cluster [name] --kubernetes-version [version] | kubectl apply -f -


$ kubectl get pods -A
NAMESPACE                           NAME                                                           READY   STATUS    RESTARTS   AGE
capi-kubeadm-bootstrap-system       capi-kubeadm-bootstrap-controller-manager-b7cb4df8b-s645x      1/1     Running   0          6m16s
capi-kubeadm-control-plane-system   capi-kubeadm-control-plane-controller-manager-cf88668b-rmp2l   1/1     Running   0          6m14s
capi-system                         capi-controller-manager-6849b49bbb-fqdcs                       1/1     Running   0          6m18s
capo-system                         capo-controller-manager-7dc9d999fc-kvvn4                       1/1     Running   0          6m12s
cert-manager                        cert-manager-848f547974-l7pnx                                  1/1     Running   0          6m45s
cert-manager                        cert-manager-cainjector-54f4cc6b5-rn5cn                        1/1     Running   0          6m45s
cert-manager                        cert-manager-webhook-7c9588c76-qjgx7                           1/1     Running   0          6m45s
kube-system                         coredns-558bd4d5db-mqs84                                       1/1     Running   0          9m31s
kube-system                         coredns-558bd4d5db-s8k4g                                       1/1     Running   0          9m31s
kube-system                         etcd-kind-control-plane                                        1/1     Running   0          9m34s
kube-system                         kindnet-8vrf7                                                  1/1     Running   0          9m31s
kube-system                         kube-apiserver-kind-control-plane                              1/1     Running   0          9m34s
kube-system                         kube-controller-manager-kind-control-plane                     1/1     Running   0          9m34s
kube-system                         kube-proxy-55pcm                                               1/1     Running   0          9m31s
kube-system                         kube-scheduler-kind-control-plane                              1/1     Running   0          9m34s
local-path-storage                  local-path-provisioner-547f784dff-fzfzc                        1/1     Running   0          9m31s  
```

* OpenStack Controller Manager가 Instance 생성시 최대 60분 대기하도록 설정
```bash
$ kubectl -n capo-system set env deployment/capo-controller-manager CLUSTER_API_OPENSTACK_INSTANCE_CREATE_TIMEOUT=60
```

### image-builder를 통한 qcow2 image 생성
```bash
// kvm 관련 패키지 설치
$ apt install qemu-kvm libvirt-bin qemu-utils

// Ubuntu 20.04 LTS 일 경우
$ apt install qemu-kvm libvirt-daemon-system libvirt-clients virtinst cpu-checker libguestfs-tools libosinfo-bin

$ sudo usermod -a -G kvm <yourusername>
$ sudo chown root:kvm /dev/kvm


$ git clone https://github.com/kubernetes-sigs/image-builder.git
$ cd image-builder/images/capi/

$ make deps-qemu

$ make build-qemu-ubuntu-1804
$ make build-qemu-ubuntu-2004


error initializing provisioner 'goss': Unknown provisioner goss
=> hack/ensure-goss.sh

$ openstack image create --disk-format qcow2 --container-format bare --public --file ./output/ubuntu-1804-kube-v1.20.10/ubuntu-1804-kube-v1.20.10 ubuntu-18.04-capi

$ openstack image create --disk-format qcow2 --container-format bare --public --file ./output/ubuntu-2004-kube-v1.20.10/ubuntu-2004-kube-v1.20.10 ubuntu-20.04-capi
```
<br/>

### Workload cluster 생성
- ssh key paire, app credential 설치  
  ```bash
  $ apt install -y python3-openstackclient
  $ openstack keypair create --private-key knit.key knit

  $ openstack application credential create cloud-controller-manager
  +--------------+----------------------------------------------------------------------------------------+
  | Field        | Value                                                                                  |
  +--------------+----------------------------------------------------------------------------------------+
  | description  | None                                                                                   |
  | expires_at   | None                                                                                   |
  | id           | 39305cccde614f0f86fa6e921e88b938                                                       |
  | name         | cloud-controller-manager                                                               |
  | project_id   | 1030e41a3f6a4d598983d715372fc375                                                       |
  | roles        | admin member reader                                                                    |
  | secret       | UAhv_AklfQu45uUTrbZUx1JEUCgR2OZJhcnijg5ybjdRO_XfGIIFiSY2m-kUYvmFYYIqXnCZSd73QTbimUs1gw |
  | system       | None                                                                                   |
  | unrestricted | False                                                                                  |
  | user_id      | 070e0441ad17402298204105a44e558e                                                       |
  +--------------+----------------------------------------------------------------------------------------+

  $ openstack application credential list
  +----------------------------------+--------------------------+----------------------------------+-------------+------------+
  | ID                               | Name                     | Project ID                       | Description | Expires At |
  +----------------------------------+--------------------------+----------------------------------+-------------+------------+
  | 39305cccde614f0f86fa6e921e88b938 | cloud-controller-manager | 1030e41a3f6a4d598983d715372fc375 | None        | None       |
  +----------------------------------+--------------------------+----------------------------------+-------------+------------+
  ```

  ```bash
  $ wget https://raw.githubusercontent.com/kubernetes-sigs/cluster-api-provider-openstack/master/templates/env.rc -O ./env.rc

  $ snap install yq  (yq가 없으면 아래 source env.rc clouds.yaml openstack 실행시 에러발생함)

  $ vi clouds.yaml  (clouds.yaml 파일 생성)
  clouds:
    openstack:
     insecure: true
     verify: false
     identity_api_version: 3
     auth:
       auth_url: http://192.168.77.11/identity
       project_name: cloudjeong
        username: cloud-jeong
        password: Math76003206
        project_domain_name: Default
        user_domain_name: Default
      region: RegionOne

  $ source env.rc clouds.yaml openstack
  // live
  $ export OPENSTACK_SSH_KEY_NAME=knit
  $ export OPENSTACK_IMAGE_NAME=ubuntu-20.04-capi
  $ export OPENSTACK_FAILURE_DOMAIN=nova
  $ export OPENSTACK_DNS_NAMESERVERS=8.8.8.8
  $ export OPENSTACK_CONTROL_PLANE_MACHINE_FLAVOR=4Core.4G
  $ export OPENSTACK_NODE_MACHINE_FLAVOR=4Core.4G

  $ openstack network list --external
  +--------------------------------------+--------+--------------------------------------+
  | ID                                   | Name   | Subnets                              |
  +--------------------------------------+--------+--------------------------------------+
  | **3bd5f7f4-da7b-4756-8609-514f16666296** | public | e410b105-65ac-4766-b56b-bb4dcf5d6b0c |
  +--------------------------------------+--------+--------------------------------------+
  $ export OPENSTACK_EXTERNAL_NETWORK_ID=3bd5f7f4-da7b-4756-8609-514f16666296


  $ clusterctl generate cluster capi-quickstart --kubernetes-version v1.23.0 --control-plane-machine-count=3 --worker-machine-count=1 > capi-quickstart.yaml
  $ kubectl apply -f capi-quickstart.yaml

  $ kubectl get cluster
  $ clusterctl describe cluster capi-quickstart
  $ kubectl get kubeadmcontrolplane

  $ clusterctl get kubeconfig capi-quickstart > capi-quickstart.kubeconfig

  $ kubectl --kubeconfig=./capi-quickstart.kubeconfig apply -f https://docs.projectcalico.org/v3.21/manifests/calico.yaml
  $ kubectl --kubeconfig=./capi-quickstart.kubeconfig get nodes
  ```

- Clean-up
```bash
$ kubectl delete cluster capi-quickstart
$ kind delete cluster
```
