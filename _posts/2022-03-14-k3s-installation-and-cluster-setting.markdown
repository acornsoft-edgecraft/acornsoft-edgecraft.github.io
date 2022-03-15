---
layout: post
title:  K3s Installation and Cluster Setting
date:   2022-03-14 10:15:35 +0300
image:  k3s.svg
tags:   edgecraft
---
# Lightweight Kubernetes인 K3S 설치 및 클러스터 세팅
***
**K3S?**  
k3s 는 경량화된 쿠버네티스로(Lightweight Kubernetes) 으로, 기본적인 Kubernetes의 기능이 제공되는 자원이 한정된 edge 환경에 매우 적합한 프로그램입니다. 라즈베리파이와 같은 아주 작은 임베디드 기기에도 충분히 잘 돌아가게 최적화가 되어 있다고 합니다.  
K3s는 Kubernetes API를 완벽하게 구현하는 40MB 미만의 단일 바이너리로 설계되었습니다. 이를 달성하기 위해 코어의 일부일 필요가 없고 애드온으로 쉽게 교체되는 추가 드라이버를 많이 제거했습니다.
리소스 요구 사항이 낮기 때문에 512MB 이상의 RAM 시스템에서 클러스터를 실행할 수 있습니다. 이는 노드뿐만 아니라 마스터에서도 포드를 실행할 수 있음을 의미합니다.

<br/>

## K3s Architecuture
- 단일 바이너리로 패키징으로 설치 간단
- Sqlite3를  기반으로 하는 경량 스토리지
- 복잡한 TLS 및 옵션 설정을 간단하게 설치
- 로컬스토리지, 서비스 로드밸런서, Helm Controller, Traefik Ingress controller 등 간단하고 강력한 “batteries-included” 기능 
- 외부 종속성 최소화 (최신 커널 및 CGROUP 마운트만 필요)
- K3s Server/ K3s Agent Node 로 구성 
- Single-Node, Multi-Node 구성 가능 

![How it Works](/images/how-it-works-k3s.svg)

## For K3s Server
K3S Server는 Kubernetes에서의 master와 같은 역할을 합니다. 
클러스터를 관리하고, 컨테이너들의 스케줄링 및 모니터링 등등의 역할을 합니다.

- API, 컨트롤러, 클라우드 컨트롤러, 스케줄러는 k3s 서버 프로세스의 일부로 시작
- K3s는 sqlite를 단일 마스터 클러스터에 대한 스토리지 지원
- Database cert bootstrap –서버는 시작 시 데이터베이스에 암호화된 인증서를 저장
- Reverse Tunnel proxy for kubelet communication –양방향 통신의 필요성 제거

## For K3S Agent
실제 컨테이너들이 주로 배포될 Agent는 Kubernetes에서의 worker node 와 같은 역할을 합니다.

- Agent 프로세스  kubelet 과  kube-proxy 를 실행
- Flannel  또한 Agent 프로세스 내에 포함
- Containerd는 컨테이너 런타임으로 실행되지만 완전히 교체 가능
- Internal load balancer -  agents 는 서버 노드에 로드밸런싱
- Network policy controller – network policies로 pod에 대한 수신 및 송신 필터링 모두 지원

## K3s Install Requirement 
- OS
  - most modern Linux systems
- DB
  - Sqlite(embedded), MySQL, PostgreSQL, etcd
  - 운영환경에서는 external database 권장   

![k3s-requirement](/images/k3s-requirement.png)

## 환경 준비
K3S를 이용해서 Local 환경에서 Cluster를 구성하는 방법을 작성해보려고 합니다.  
하드웨어 요구 사항은 배포 규모에 따라 확장됩니다. 여기에는 최소 권장 사항이 설명되어 있습니다.

RAM: 최소 512MB(최소 1GB 권장)
CPU: 최소 1개

- VirtualBox
- VirtualBox 내에서 Raspberry Pi Desktop 설치
  - cpu: 1core
  - memory: 1024MB
- VirtualBox 내에서 Raspberry Pi Desktop 설치
  - cpu: 1core
  - memory: 512MB



## k3s Server 설치
K3s는 systemd 또는 openrc 기반 시스템에 서비스로 설치하는 편리한 방법인 설치 스크립트를 제공합니다. 이 스크립트는 https://get.k3s.io 에서 사용할 수 있습니다. 
이 방법을 사용하여 K3를 설치하려면 다음을 실행하십시오.

- Install Script
    ```sh
    $ curl -sfL https://get.k3s.io | sh -

    ## k3s 서비스 확인
    $ sudo systemctl status k3s

    ## 클러스터 확인
    $ kubectl get nodes
    ```

- 이 설치를 실행한 후:
  - K3s 서비스는 노드 재부팅 후 또는 프로세스가 충돌하거나 종료된 경우 자동으로 다시 시작하도록 구성됩니다.
  - 추가 유틸리티가 설치됩니다. (**kubectl**, **crictl**, **ctr**, **k3s-killall.sh**, 및 **k3s-uninstall.sh**)
  - kubeconfig 파일은 /etc/rancher/k3s/k3s.yaml에 작성되고 K3s에 의해 설치된 kubectl은 자동으로 이를 사용합니다.

## k3s Agent 설치
작업자 노드에 설치하고 클러스터에 추가하려면 K3S_URL 및 K3S_TOKEN 환경 변수를 사용하여 설치 스크립트를 실행하십시오. 다음은 작업자 노드에 참여하는 방법을 보여주는 예입니다.  
 **참고:** 각 시스템에는 고유한 호스트 이름이 있어야 합니다. 시스템에 고유한 호스트 이름이 없으면 K3S_NODE_NAME 환경 변수를 전달하고 각 노드에 대해 유효하고 고유한 호스트 이름을 값으로 제공하십시오.

- Install Script
  - K3S_URL 매개변수를 설정하면 K3가 작업자 모드에서 실행됩니다. K3s 에이전트는 제공된 URL에서 수신 대기하는 K3s 서버에 등록합니다. 
  - K3S_TOKEN에 사용할 값은 서버 노드의 /var/lib/rancher/k3s/server/node-token에 저장됩니다.

```sh
$ curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -
```

-----
**참고**
- [K3s - Lightweight Kubernetes](https://rancher.com/docs/k3s/latest/en/)  
- [[KCD KOREA 2021] Sponsor Session \| IoT 및 Edge 컴퓨팅을 대비한 K3s 구축, Applicaion 개발 및 모니터링 \| 남궁성환](https://www.youtube.com/watch?v=vG36HwTiDks&list=PLj6h78yzYM2OO9_EWXS13LxAe-Bkn0xXt&index=3&t=893s)