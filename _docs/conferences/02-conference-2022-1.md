---
layout: doc
menu: 2차 컨퍼런스 (2022년)
title: Edgecraft 2차 컨퍼런스 (2022년)
category: 커뮤니티
order: 1
---

<!-- <div class="page__content" style="padding: 0 80px"> image와 폭 맞춤을 위한 스타일 설정
</div> -->

## Contacts

- slack : edgecraft-community.slack.com
- email : edgecraft@acornsoft.io

<p align="center"><img src="/images/conference-02.png"></p>

# 엣지 클라우드 구축 플랫폼을 위한 Cluster API 적용 사례

<br/>

## Session 1 : Cluster API 이해

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

### Concepts

  <p align="center">
    <img style="margin: 0" src="/images/capi_architecture.png">
    Cluster API Concepts
  </p>

Cluster API는 Kubernetes 클러스터의 Provisioning, Upgrade 및 Operation을 단순화하기 위한 선언적 API 및 도구를 제공하고 Kubernetes Cluster (Workload Cluster)의 라이프사이클을 관리하는 기능을 제공한다.

- Management Cluster  
  위의 그림과 같이 Controllers 와 CRD 가 설치된 클러스터를 의미한다.
- Workload Cluster  
  CR에 대한 선언적 변경 (또는 신규)이 발생하면 Controller의 Reconcile 함수가 동작하면서 관리되는 최종 클러스터를 의미한다.

### Components

- Cluster API CRDs
  쿠버네티스에서 제공하는 기본 리소스들과는 별개로 Cluster 라이프사이클 관리를 위해 사용자 정의된 리소스들로 Custom Resources + Controller로 구성
  - Cluster  
    Kubernetes Cluster 구성
  - Machine  
    Kubernetes Cluster 내의 Node에 대한 구성
  - MachineSet  
    같은 형상을 가지는 Machine 그룹
  - MachineDeployment  
    MachineSet을 만들고 관리하며 Machine에 대한 Scale/Update 기능
  - MachineHealthCheck  
    Machine 상태 확인
- Controllers
  Custom Resource의 변동 사항을 수신해서 실제 클러스터의 라이프사이클을 관리하며, Cluster API 에서는 각 Provider들을 연동하여 동작
  - Bootstrap Controller
    - Cluster Bootstrap
    - Machine Bootstrap (Cluster 내 Node)
  - Cluster Controller
    ```shell
    kubectl get clusters --kubeconfig [KUBECONFIG]
    ```
    - Cluster 리소스를 만들고 관리
    - Cluster 구성
      - graceful deletion
    - Cluster actuator를 호출하여 reconcilation 처리
    - Cluster actuator를 호출하여 삭제
  - Machine Controller
    ```shell
    kubectl get machines --kubeconfig [KUBECONFIG]
    ```
    - Cluster 내의 Node 설정
    - Machine 리소스를 만들고 관리
    - Machine들 구성
      - graceful deletion
      - proper deletion order
      - cascading deletion
    - Machine actuator를 호출하여 생성/업데이트/삭제
  - MachineSet Controller
    - 동일한 형상을 가지는 Machine들의 그룹
    - ReplicaSet과 비슷 (Pod 대신 Machine)
    - MachineSet 리소스들을 만들고 관리
    - MachineSet을 구성
      - proper deletion order
      - cascading deletion
    - 분리된 Machine 사용
  - MachineDeployment Controller
    ```shell
    kubectl get machinedeployments --kubeconfig [KUBECONFIG]
    ```
    - deployment와 비슷
    - MachineDeployment 리소스를 만들고 관리
    - MachineDeployment 리소스 구성
      - proper deletion
      - cascading deletion
    - MachineSet 만들고 관리
      - Rolling update
        - max unavailable  
          허용할 수 있는 비활성 machine의 수 설정
        - max surge  
          원하는 replicas 이상으로 허용할 수 있는 extra machines 수 설정
    - 분리된 MachineSet 사용
  - MachineHealthCheck Controller
    - Unhealthy Machine 관리
      - Unhealthy 조건을 비교해서 Workload Cluster의 Node 확인
      - Unhealthy 상태인 Node 개선
  - ControlPlane Controller
    - Kubernetes Control Plane에 해당하는 MachineSet 관리
    - Control Plane 정보를 downstream customers에 제공
    - Workload Cluster에 액세스하기 위한 kubeconfig file을 secret으로 생성/관리
- Infrastructure Provider (Openstack)
  - AWS, Azure, Google 등의 퍼블릭 클라우드와 VMWare, metak3.io, Openstack 등의 VM Provider에서 제공해 주는 리소스 관리용 Provider
  - Provider는 Infrastructure에 직접 연계하는 것이기 때문에 별도의 CRDs를 가진다.
    - OpenstackCluster
    - OpenstackMachine
    - OpenstackMachineTemplate
    - OpenstackClusterTemplate
- Bootstrap Provider
  cloud-init과 같이 Cluster Node의 Bootstrap 작업 처리 주체 (kubeadm 사용)
  - KubeadmConfig
  - KubeadmConfigTemplate
- Control Plane Provider
  - KubeadmControlPlane
  - KubeadmControlPlaneTemplate

### Processing

<p align="center">
  <img style="margin: 0" src="/images/capi_processing.png">
  Cluster API Processing Concept
</p>

</div>
</details>

<br/>

## Session 2 : 대외 소개 자료 및 데모

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

<p align="center">
<strong>SK Telecom, ICT 기술센터, CloudLab 김에스더님의 Cluster API 소개(Slideshare)</strong>

<blockquote class="embedly-card"><h4><a href="https://www.slideshare.net/EstherKim194/cluster-api-koss-2019-202194975">Cluster api - koss 2019</a></h4><p>1. Motivation of Cluster API Project 2. Cluster API Introduction & Components 3. Cluster API Provider (CAPO)</p></blockquote>
<script async src="//cdn.embedly.com/widgets/platform.js" charset="UTF-8"></script>
</p>

<p align="center">
<strong>Cluster API Demo</strong>
<script id="asciicast-DaMKYCSaHWuqFq1l4pEAZMKsR" src="https://asciinema.org/a/DaMKYCSaHWuqFq1l4pEAZMKsR.js" async></script>
</p>

</div>
</details>

<br/>

## Session 3 : Edgecraft 적용 방식

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

![Edgecraft 적용 방식](/images/edgecraft-capi-flow.png)

1. 사용자가 생성할 클러스터 정보를 관리 UI에 설정하고 서버로 전송
2. API Server는 전송된 정보를 Database에 저장  
   2.1 Provision인 경우는 3번으로 진행  
   2.2 저장 Only인 경우는 6번으로 진행
3. Go-Template을 이용해서 전송된 정보를 Cluster API에 맞는 Manifests 정보로 변환
   3.1. Create Cluster manifest
   3.2. Create Master/Worker MachineSet manifests
   3.3. Update scale-in/out to Custom Reosurce
4. Kubernetes client (client-go dynamic)를 이용해서 Manifest 정보를 적용 (Provisioning 단계)  
   3.1. Cluster API CR 정보 생성 또는 갱신 발생  
   3.2. Cluster API Controller들의 Reconcile 호출을 통해 Provider CR 정보 설정  
   3.3. Provier Controller들의 Reconcile 호출을 통해 Openstack의 관련된 작업 진행
5. Provisioning 검증 Worker 실행 (Backgroud Workers)  
   5.1. 클러스터에 대한 kubeconfig 생성 여부 검증  
   5.2. 클러스터의 Provision 상태 검증  
   5.3. Master 또는 Worker MachineSet인 경우는 Node 생성 여부 검증  
   5.4. 기타 업무에 따른 검증  
   5.5. 검증된 결과를 Database에 Update
6. 요청 결과 반환

</div>
</details>

<br/>
