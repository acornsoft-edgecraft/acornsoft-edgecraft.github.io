---
layout: post
title:  Install MicroK8s and Clustering
date:   2022-03-14 11:40:35 +0300
image:  microk8s-logo.png
tags:   edgecraft # 수정가능성 있음
---
# MicroK8s 소개
 MicroK8s는 업스트림 릴리스를 추적하고 클러스터링을 간단하게 만드는 가장 작고 빠르며 완전히 호환되는 Kubernetes입니다. 
 MicroK8s는 오프라인 개발, 프로토타이핑 및 테스트에 적합합니다. VM에서 작고 저렴하며 안정적인 CI/CD용 k8s로 사용하십시오. 또한 어플라이언스를 위한 최고의 프로덕션 등급 Kubernetes입니다. MicroK8s에 k8s IoT Edge 앱을 개발하고 배포하십시오.

## Bare-Metal Kubernetes에 대한 의사 결정 트리
![에지의 Bare-Metal Kubernetes에 대한 의사 결정](/images/choose-bare-metal-kubernetes.png)

- [MicroK8s](https://microk8s.io/docs): 관리 및 구성 간소화("LowOps"), Canonical의 준수 Kubernetes.
- [K3s](https://rancher.com/docs/k3s/latest/en/architecture/): IoT(사물 인터넷) 및 에지 컴퓨팅용으로 빌드된 인증된 Kubernetes 배포입니다.
- [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm): Kubernetes 클러스터를 만들기 위한 Kubernetes 도구입니다. 표준 컴퓨팅 리소스(Linux/Windows)에 적합합니다.

### 고려 사항
1. 운영 비용: Kubernetes 클러스터를 유지 관리하고 운영하는 데 필요한 예상 인건비입니다.
   - 관리형 서비스와 함께 제공되는 지원이 없으면 조직에서 클러스터를 전체적으로 유지 관리하고 운영해야 합니다(스토리지, 네트워킹, 업그레이드, 관찰 가능성, 애플리케이션 관리). 운영 비용은 높은 것으로 간주됩니다.
 
2. 구성 용이성: Kubernetes 클러스터를 구성하고 배포하기 어려운 수준입니다.
   - 네트워킹, 스토리지 또는 모니터링 옵션 중에서 구성의 모든 단계에서 다양한 오픈 소스 옵션을 평가하는 것은 피할 수 없으며 복잡해질 수 있습니다. 클러스터 구성을 위해 CI/CD를 구성하려면 더 많은 고려 사항이 필요합니다. 이러한 문제로 인해 구성의 용이성을 어렵게 간주합니다.

3. 유연성: Kubernetes 옵션을 얼마나 조정할 수 있는지에 대한 측정값은 사용자 지정된 구성을 에지의 기존 인프라와 통합하는 것입니다.
   - 공급자 제한 없이 오픈 소스 도구 또는 플러그인을 사용할 수 있는 기능을 통해 운영형 Kubernetes는 매우 유연합니다.

4. 혼합 노드: Linux 및 Windows 노드를 모두 사용하여 Kubernetes 클러스터를 실행할 수 있습니다.

**참고:** 낮은 Ops는 자동 업데이트 또는 간소화된 업그레이드와 같이 일부 운영 작업이 추상화되거나 더 쉽게 만들어질 때 작업 비용이 감소하는 것을 의미합니다.

## 필요한 것
- 명령을 실행하기 위한 Ubuntu 20.04 LTS, 18.04 LTS 또는 16.04 LTS 환경(또는 snapd를 지원하는 다른 운영체제)
- 최소 20G의 디스크 공간과 4G의 메모리가 권장됩니다.

## MicroK8s 설치
MicroK8s는 거의 모든 시스템에서 실행하고 사용할 수 있는 최소한의 경량 Kubernetes를 설치합니다. snap으로 설치할 수 있습니다.
1. CentOS 8에 EPEL 추가  
    다음 명령을 사용하여 EPEL 리포지토리를 CentOS 8 시스템에 추가 할 수 있습니다.  
    ```sh  
    $ sudo yum install -y epel-release

    ## 방화벽 정지는 기본입니다.  
    $ service firewalld stop
    ```
<br/>
2. 스냅 설치  
    EPEL 리포지토리를 CentOS 설치에 추가한 다음 스냅된 패키지를 설치하십시오 .
    ```sh
    $ sudo yum install -y snapd

    ## 설치 후에는 systemd 주요 스냅 통신 소켓 요구를 관리 장치는 사용 가능합니다 :
    $ sudo systemctl enable --now snapd.socket

    ## 클래식 스냅 지원을 활성화하려면 다음을 입력하여 /var/lib/snapd/snap와 사이에 기호 링크를 만듭니다 /snap.
    $ sudo ln -s /var/lib/snapd/snap /snap
    ```
    <br/>
3. Install MicroK8s  
    MicroK8s는 거의 모든 시스템에서 실행하고 사용할 수 있는 최소한의 경량 Kubernetes를 설치합니다. 스냅으로 설치할 수 있습니다.
    ```sh
    $ sudo snap install microk8s --classic --channel=1.21/stable
    2021-09-06T07:39:40Z INFO Waiting for automatic snapd restart...
    microk8s (1.21/stable) v1.21.4 from Canonical✓ installed
    ```

    <br/>
4. 그룹에 가입  
    MicroK8s는 관리자 권한이 필요한 명령을 원활하게 사용할 수 있도록 그룹을 생성합니다. 현재 사용자를 그룹에 추가하고 .kube 캐싱 디렉토리에 대한 액세스 권한을 얻으려면 다음 두 명령을 실행하십시오.
    ```sh
    $ sudo usermod -a -G microk8s $USER
    $ sudo chown -f -R $USER ~/.kube

    ## 또한 그룹 업데이트를 수행하려면 세션을 다시 시작해야 합니다.
    $ su - $USER
    ```
    <br/>
5. 상태 확인  
    MicroK8s에는 상태를 표시하는 명령이 내장되어 있습니다. 설치하는 동안 --wait-ready플래그를 사용하여 Kubernetes 서비스가 초기화될 때까지 기다릴 수 있습니다 .
    ```sh
    $ microk8s status --wait-ready
    ```
    <br/>
6. Kubernetes에 액세스  
    MicroK8s는 kubectlKubernetes에 액세스하기 위한 자체 버전을 번들로 제공합니다 . Kubernetes를 모니터링하고 제어하는 ​​명령을 실행하는 데 사용합니다. 예를 들어 노드를 보려면 다음을 수행합니다.
    ```sh
    $ microk8s kubectl get nodes

    ## ...또는 실행 중인 서비스를 보려면:
    $ microk8s kubectl get services
    ```
    <br/>
7. 앱 배포  
    물론 Kubernetes는 앱과 서비스를 배포하기 위한 것입니다. 다른 Kuberenetes와 마찬가지로 kubectl 명령을 사용하여 이를 수행 할 수 있습니다.
    ```sh
    $ microk8s kubectl create deployment nginx --image=nginx
    ```
    <br/>
8. 추가 기능 사용  
    MicroK8s는 순수하고 가벼운 Kubernetes를 위해 최소한의 구성 요소를 사용합니다. 그러나 간단한 DNS 관리에서 Kubeflow를 사용한 기계 학습에 이르기까지 Kubernetes에 추가 기능을 제공할 사전 패키지 구성 요소인 "추가 기능"을 사용하여 몇 번의 키 입력으로 많은 추가 기능을 사용할 수 있습니다!  
    시작하려면 서비스 간 통신을 용이하게 하기 위해 DNS 관리를 추가하는 것이 좋습니다. 스토리지가 필요한 애플리케이션의 경우 '스토리지' 추가 기능이 호스트에 디렉토리 공간을 제공합니다. 다음은 설정하기 쉽습니다.
    ```sh
    ## CoreDNS 애드온을 활성화
    $ microk8s enable dns storage

    ## 이러한 추가 기능은 다음 disable명령을 사용하여 언제든지 비활성화할 수 있습니다 
    $ microk8s disable dns

    ## 사용 가능하고 설치된 애드온 목록을 확인할 수 있습니다 .
    $ microk8s status

    microk8s is running
    high-availability: no
    datastore master nodes: 127.0.0.1:19001
    datastore standby nodes: none
    addons:
    enabled:
        dns                  # CoreDNS
        ha-cluster           # Configure high availability on the current node
        storage              # Storage class; allocates storage from host directory
    disabled:
        ambassador           # Ambassador API Gateway and Ingress
        cilium               # SDN, fast with full network policy
        dashboard            # The Kubernetes dashboard
        fluentd              # Elasticsearch-Fluentd-Kibana logging and monitoring
        gpu                  # Automatic enablement of Nvidia CUDA
        helm                 # Helm 2 - the package manager for Kubernetes
        helm3                # Helm 3 - Kubernetes package manager
        host-access          # Allow Pods connecting to Host services smoothly
        ingress              # Ingress controller for external access
        istio                # Core Istio service mesh services
        jaeger               # Kubernetes Jaeger operator with its simple config
        keda                 # Kubernetes-based Event Driven Autoscaling
        knative              # The Knative framework on Kubernetes.
        kubeflow             # Kubeflow for easy ML deployments
        linkerd              # Linkerd is a service mesh for Kubernetes and other frameworks
        metallb              # Loadbalancer for your Kubernetes cluster
        metrics-server       # K8s Metrics Server for API access to service metrics
        multus               # Multus CNI enables attaching multiple network interfaces to pods
        openebs              # OpenEBS is the open-source storage solution for Kubernetes
        openfaas             # openfaas serverless framework
        portainer            # Portainer UI for your Kubernetes cluster
        prometheus           # Prometheus operator for monitoring and logging
        rbac                 # Role-Based Access Control for authorisation
        registry             # Private image registry exposed on localhost:32000
        traefik              # traefik Ingress controller for external access
    ```
    **참고:** 추가 클라이언트 도구(예: Helm)를 설치하는 추가 기능을 설치하기 전에 현재 사용자가 microk8s그룹의 일부인지 확인하여 잠재적인 권한 문제를 방지하십시오.  
    [애드온 전체 목록 보기](https://microk8s.io/docs/addons#heading--list)
    <br/>
9. MicroK8s 시작 및 중지  
    다음과 같은 간단한 명령으로 MicroK8s을 중지하고 시작할 수 있습니다.
    ```sh
    ## MicroK8s 및 해당 서비스를 중지합니다
    $ microk8s stop

    ## MicroK8s 및 해당 서비스를 중지합니다
    $ microk8s start
    ```
<br/><br/><br/>

# Clustering with MicroK8s
---
**참고:** MicroK8s 클러스터의 각 노드는 작동할 자체 환경이 필요합니다. 단일 시스템의 별도 VM이든 컨테이너이든 동일한 네트워크의 다른 시스템이든 상관 없습니다. 거의 모든 네트워크 서비스와 마찬가지로 노드 간 통신이 작동하려면 이러한 인스턴스가 올바른 시간(예: ntp 서버에서 업데이트됨)을 갖는 것도 중요합니다.


## Asia/Seoul 로 시스템 시간 설정 
```bash
## 1. 기존 서버 정보를 주석 처리하고 한국과 아시아를 추가한다.
$ vi /etc/chrony.conf
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server time.bora.net iburst
server send.mx.cdnetworks.com iburst

## 2. 변경 사항을 반영하기 위해 chronyd 를 재구동한다
$ sudo systemctl restart chronyd

## 3. RTC time을 UTC로 사용 한다.
$ timedatectl set-local-rtc 0

## 4. timezone을 Asia/Seoul로 설정 한다.
$ timedatectl set-timezone Asia/Seoul

# set-timezone 명령 실패 시 해당 cmd로 재설정 한다.
$ timedatectl set-local-rtc 0
$ timedatectl set-timezone Asia/Seoul
Failed to set time zone: Failed to update /etc/localtime
$ ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```
<br/>

## 노드 추가

클러스터를 생성하려면 둘 이상의 이미 실행 중인 MicroK8s 인스턴스에서 microk8s add-node명령을 사용합니다. 이 명령이 실행되는 MicroK8s 인스턴스는 클러스터의 마스터가 되며 Kubernetes 제어 평면을 호스팅합니다.
```sh
## 클러스터의 마스터가 되는 노드 에서 아래 명령 실행
$ microk8s add-node

## 그러면 다음과 같은 일부 가입 지침이 반환됩니다.
Join node with:
microk8s join ip-172-31-20-243:25000/DDOkUupkmaBezNnMheTBqFYHLWINGDbf

If the node you are adding is not reachable through the default
interface you can use one of the following:

microk8s join 10.1.84.0:25000/DDOkUupkmaBezNnMheTBqFYHLWINGDbf
microk8s join 10.22.254.77:25000/DDOkUupkmaBezNnMheTBqFYHLWINGDbf

## 클러스터 에 가입하려는 MicroK8s 인스턴스에서 반환 된 microk8s join 명령 실행
$ microk8s join ip-172-31-20-243:25000/DDOkUupkmaBezNnMheTBqFYHLWINGDbf

## 노드 확인
$ microk8s kubectl get no
NAME         STATUS   ROLES    AGE   VERSION
microk8s-1   Ready    <none>   22h   v1.21.4-3+e5758f73ed2a04
microk8s-2   Ready    <none>   14s   v1.21.4-3+e5758f73ed2a04
```
<br/>

## 노드 제거
먼저 제거하려는 노드에서 **microk8s leave** 명령을 실행 합니다. MicroK8s는 컨트롤 플레인(API 서버)을 다시 시작하고 전체 단일 노드 클러스터로 작업을 재개합니다.
```sh
$ microk8s leave

## 노드 제거를 완료하려면
$ microk8s remove-node 10.22.254.79
```
<br/>

## 고가용성
MicroK8s의 1.19 릴리스부터 HA는 기본적으로 활성화되어 있습니다. 클러스터가 3개 이상의 노드로 구성된 경우 데이터 저장소는 노드 전체에 복제되고 단일 장애에 대한 복원력이 있습니다(하나의 노드에서 문제가 발생하면 워크로드가 중단 없이 계속 실행됨).
```sh
## microk8s status로 확인 가능
$ microk8s status

microk8s is running
high-availability: yes
  datastore master nodes: 10.128.63.86:19001 10.128.63.166:19001 10.128.63.43:19001
  datastore standby nodes: none
```
-----
**참고:**
 - [Introduction to MicroK8s](https://microk8s.io/docs)  
 - [MicroK8s Add ons](https://microk8s.io/docs/addons)  
 - [Edge 플랫폼 옵션에서 운영체제 미설치 Kubernetes 선택](https://docs.microsoft.com/ko-kr/azure/architecture/operator-guides/aks/choose-bare-metal-kubernetes)
 - [HA 클러스터](https://microk8s.io/docs/high-availability)