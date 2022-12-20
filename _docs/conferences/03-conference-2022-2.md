---
layout: doc
menu: 3차 컨퍼런스 (2022년)
title: EdgeCraft 3차 컨퍼런스 (2022년)
category: 커뮤니티
order: 1
---

<!-- <div class="page__content" style="padding: 0 80px"> image와 폭 맞춤을 위한 스타일 설정
</div> -->

## Contacts

- slack : edgecraft-community.slack.com
- email : edgecraft@acornsoft.io

<p align="center"><img src="/images/conference-03.png"></p>

# 엣지 클라우드 구축 플랫폼 프로토타입 시제품 소개

- 2차년도의 엣지 클라우드 구축 플랫폼 프로토타입은 Openstack 기반의 클라우드에 클러스터를 Cluster API 연동을 통해 관리하는 기능을 검증하는 차원의 시제품이다.
- 2차년도의 엣지 클라두으 구축 플랫폼 디자이너 프로토타입은 인프라, 클라우드, 플랫폼 어플리케이션 탑재를 위한 기본 기능을 검증하는 차원의 시점이다.

<br />

## Session 1 : API Server Swagger 기준 API

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

- Cloud 관련 APIs  
  Cloud는 Baremetal 과 Openstack을 사용하는 2가지 유형으로 구성된다.
  ![Cloud](/images/swagger-cloud.png)
- Cloud를 구성하는 Node 관련 APIs  
  각 Cloud를 구성하는 Node들은 Master / Worker로 구성된다.
  ![Node](/images/swagger-node.png)
- Openstack Cluster 관련 APIs  
  Openstack Cloud 내에 Kubernetes Cluster 관련 APIs
  ![cluster](/images/swagger-openstack-cluster.png)
- Openstack Cluster를 구성하는 NodeSet 관련 APIs  
  NodeSet은 MasterSet, WorkerSet으로 구분되며, 각 Set은 Replicas를 통해 실제 운용할 Node의 수를 지정할 수 있으며 MasterSet은 1개만 허용되고, WorkerSet은 여러 개를 운영할 수 있다.
  ![NodeSet](/images/swagger-nodeset.png)

</div>
</details>

<br/>

### 관리 UI

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

#### Content here!!!

</div>
</details>

<br/>

### 디자이너 UI

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

![Designer Screen](/images/designer-screen.png)

디자이너 프로토타입은 아래와 같은 영역 구조를 가진다.

- 메뉴 영역
  - 인프라 디자이너 (Cloud)
  - 클러스터 디자이너 (Cluster)
  - 플랫폼 디자이너 (Platform Applications)
- 컴포넌트 영역
  - Cloud Types
  - Cluster Components
  - Networks
  - Storage Types
  - 향후 용도에 맞는 컴포넌트들 추가
- 배치 디자인 영역
  - 컴포넌트 영역에서 사용할 컴포넌트 Drag & Drop 으로 배치
  - 각 컴포넌트 선택 후 Moving
  - 전체 디자인 뷰 (오른쪽 하단)
  - 확대/축소, 화면 View Fitting, Lock 제어 버튼 (왼쪽 하단)
  - 재 배치, 전채 선택, 저장, 로드 명령 버트 (오른쪽 상단)
  - 배치된 컴포넌트간의 연결선 Drawing
- 컴포넌트 속성 영역
  - 디자인 영역에 배치된 컴포넌트에 대한 속성 정보 설정
  - 향후 연결선을 통해 연결된 컴포넌트간의 연결 정보 설정 기능 추가 예정

사용자에 의해서 디자인된 인프라, 클러스터, 플랫폼 어플리케이션 디자인 정보는 디자인 영역의 "Save, Load" 버튼에 의해서 JSON 데이터 관리된다.

![Designer JSON Data](/images/designer-json-data.png)

위의 그림과 같이 배치된 디자인 영역을 "저장"하면 우선 브라우저의 LocalStorage에 관련된 JSON 데이터를 저장한다.
이 부분은 향후 서버에서 템플릿으로 관리하는 방식으로 변경 적용될 예정이다.

디자인된 배치 정보는 크게 다음과 같이 구성된다.

- 디자이너 데이터
  - Position
  - Zoom
- 컴포넌트 연계 데이터
  - nodes - 컴포넌트 자체 데이터
    - 디자이너 정보
    - data - 컴포넌트 속성 정보를 가진다.
  - edges - 컴포넌트가 연결 데이터
    - sourceNode - 연결 소스 컴포넌트
    - targetNode - 연결 대상 컴포넌트

기본적인 사용 방법은 아래의 동영상 참고

<iframe width="560" height="315" src="https://www.youtube.com/embed/UQ8MPIeKXis" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

</div>
</details>

<br/>
