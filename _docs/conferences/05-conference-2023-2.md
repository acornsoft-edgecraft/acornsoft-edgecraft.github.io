---
layout: doc
menu: 5차 컨퍼런스 (2023년)
title: EdgeCraft 5차 컨퍼런스 (2023년)
category: 커뮤니티
order: 1
---

<!-- <div class="page__content" style="padding: 0 80px"> image와 폭 맞춤을 위한 스타일 설정
</div> -->

## Contacts

- slack : edgecraft-community.slack.com
- email : edgecraft@acornsoft.io

<p align="center"><img src="/images/conference-05.png"></p>

# 엣지 클라우드 구축 플랫폼 백업/복원 시스템 소개

- 3차년도의 엣지 클라우드 구축 플랫폼 엣지 클라우드 백업/복원 시스템은 Velero를 사용하여 다음 기능을 수행할 수 있습니다.
  - 클러스터를 백업하고 필요할 때 복구를 할 수 있습니다.
  - 클러스터 자원을 다른 클러스터로 마이그레이션 할 수 있습니다.
  - 클러스터를 복제할 수 있습니다.

<br />

## Session 1 : Velero란 무엇인가?

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

### Velero란 무엇인가요?

Velero는 Kubernetes 클러스터에서 애플리케이션 및 클러스터 상태의 백업 및 복원을 지원하는 오픈 소스 도구입니다. Velero는 이러한 기능을 제공하여 Kubernetes 환경에서 데이터 손실 및 재해 복구 시나리오에 대비할 수 있도록 도와줍니다.

Velero는 쿠버네티스 클러스터의 안전성과 신뢰성을 높이기 위한 중요한 도구 중 하나이며, 데이터 손실을 방지하고 재해 복구를 위한 백업 및 복원 솔루션으로 사용됩니다. Velero는 CNCF (Cloud Native Computing Foundation) 프로젝트 중 하나로서, Kubernetes의 생태계와 통합되어 있으며 커뮤니티에 의해 지속적으로 개발되고 유지보수됩니다.


### Velero의 기능 및 주요 컨셉은 다음과 같습니다

- 데이터 손실 방지: Kubernetes 클러스터 내의 애플리케이션 및 데이터는 중요합니다. Velero를 사용하여 백업을 수행하면 애플리케이션 데이터를 보호하고 데이터 손실을 방지할 수 있습니다.

- 재해 복구: 클러스터에 장애가 발생했을 때, Velero를 사용하여 클러스터를 빠르게 복구할 수 있습니다. 이것은 장애 복구 시간을 단축하고 비즈니스 연속성을 유지하는 데 도움이 됩니다.

- 테스트 및 개발: Velero는 개발 및 테스트 환경에서 사용할 수 있으므로, 개발자는 클러스터를 쉽게 되돌릴 수 있고, 새로운 기능을 시험하거나 버그를 디버깅할 수 있습니다.

- 이동 및 마이그레이션: Velero는 클러스터 간 또는 클라우드 프로바이더 간에 애플리케이션과 데이터를 마이그레이션하는 데 사용할 수 있습니다. 이것은 클라우드 벤더 변경 또는 환경 이동 시 유용합니다.

- 정책 관리: Velero는 백업 및 복원 정책을 정의하고 관리할 수 있어서 특정 애플리케이션이나 네임스페이스에 대한 백업 정책을 설정할 수 있습니다.

- 오픈 소스 및 확장 가능성: Velero는 CNCF(클라우드 네이티브 컴퓨팅 재단)의 일부로서 오픈 소스로 개발되어 커뮤니티에 의해 지속적으로 발전하고 있습니다. 또한 플러그인 아키텍처를 지원하여 다양한 스토리지 백엔드 및 기타 확장 기능을 추가할 수 있습니다.

![Velero Backup Workflow](/images/velero-workflow.png)


</div>
</details>
<br/>

## Session 2 : 엣지 클라우드 백업/복원 시스템 소개 및 데모

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

##### 1. 엣지 클라우드 백업/복원 시스템 소개

Edgecraft 백업/복원 시스템은 분산되어 있는 클러스터들의 백업이 필요한 경우 Velero 플러그인을 사용하여 Object Storage에 백업할 수 있습니다. 이를 통합 관리 하기 위해 Cluster API를 활용하여 관리 클러스터에서 해당 클러스터들의 백업/복원 절차를 통합 관리 할 수 있습니다.

- 사전 준비
Object Storage installed - Minio

<br/>

- Work flow

![Edgecraft BackRes Workflow](/images/edgecraft-backres-workflow.png)

<br/>

##### 2. 엣지 클라우드 백업/복원 시스템 데모

- Target cluster backup and restore
  <br/>
  <iframe width="560" height="315" src="https://www.youtube.com/embed/bkZNvtoTrDg?si=b5UEq0-KNI49VgtA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
  <br/>
  <br/>


<br/>
