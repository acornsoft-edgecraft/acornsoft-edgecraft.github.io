---
layout: doc
menu: 4차 컨퍼런스 (2023년)
title: EdgeCraft 4차 컨퍼런스 (2023년)
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

- 3차년도의 엣지 클라우드 구축 플랫폼 엣지 클라우드 백업/복원 시스템은 관리하는 작업을 자동화 하는 시스템 이다.
- 3차년도의 엣지 클라우드 구축 플랫폼 엣지 클라우드 무결성(보안, 안정성) 검증 자동화 시스템은 KaaS 지원 유형(kubeadm, k3s, microk8s)을 지원 합니다.

<br />

## Session 1 : CIS Benchmarks란 무엇인가?

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

### CIS 벤치마크란 무엇인가요?

Center of Internet Security(CIS)의 CIS 벤치마크는 세계적으로 인정받는 일련의 합의 기반 모범 사례로, 보안 전문가가 사이버 보안 방어를 구현하고 관리하도록 지원합니다. 보안 전문가로 구성된 글로벌 커뮤니티가 참여를 통해 개발한 이 지침은 새로운 위험으로부터 조직을 사전 예방적으로 보호할 수 있도록 지원합니다. 기업들은 디지털 자산에서 구성 기반 보안 취약성을 최소화하기 위해 CIS 벤치마크 지침을 구현합니다.


### CIS 벤치마크가 중요한 이유는 무엇인가요?

CIS 벤치마크와 같은 도구는 25개 이상의 공급업체 제품을 배포하기 위해 보안 전문가와 분야별 전문가가 개발한 보안 모범 사례를 개괄적으로 설명하기 때문에 중요합니다. 이러한 모범 사례는 새로운 제품 또는 서비스의 배포 계획을 수립하거나 기존 배포가 안전한지 확인하기 위한 좋은 출발점이 됩니다.

CIS 벤치마크를 구현할 때, 다음과 같은 단계를 수행하여 일반 리스크와 새롭게 등장하는 리스크로부터 레거시 시스템을 보다 효과적으로 보호할 수 있습니다. 

- 사용되지 않는 포트 비활성화
- 불필요한 앱 권한 제거
- 관리 권한 제한

또한 IT 시스템과 애플리케이션은 불필요한 서비스를 사용하지 않도록 설정할 때 더 나은 성능을 발휘합니다. 

CIS 벤치마크를 도입하면 다음과 같은 몇 가지 사이버 보안 관련 이점을 얻을 수 있습니다.
 
- 전문가 사이버 보안 지침
  - CIS 벤치마크는 전문가의 검증을 통해 입증된 보안 구성 프레임워크를 조직에 제공합니다. 기업은 보안의 위험을 초래하는 시행착오 시나리오를 피하면서 다양한 IT 및 사이버 보안 커뮤니티의 전문 지식을 활용할 수 있습니다.


- 세계적으로 인정받는 보안 표준
  - CIS 벤치마크는 전 세계의 정부, 기업, 연구 및 학술 기관 모두가 인정하고 수용하는 유일한 모범 사례 가이드입니다. 합의 기반 의사 결정 모델을 기반으로 하는, 글로벌하고 다양한 커뮤니티 덕분에 CIS 벤치마크는 리전별 법률 및 보안 표준보다 훨씬 폭넓은 적용 범위와 수용 범위를 자랑합니다. 


- 비용 효율적인 위협 예방
  - CIS 벤치마크 문서는 누구나 무료로 다운로드하여 구현할 수 있습니다. 기업이 모든 종류의 IT 시스템에 대한 최신 단계별 지침을 무료로 얻을 수 있습니다. IT 거버넌스를 실현하고 예방 가능한 사이버 위협으로 인한 재정적 피해와 평판의 손상을 방지할 수 있습니다.


- 규제 준수
  - CIS 벤치마크는 다음과 같은 주요 보안 및 데이터 프라이버시 프레임워크에 부합합니다.
    - 미국 국립 표준 기술 연구소(NIST) 사이버 보안 프레임워크 
    - Health Insurance Portability and Accountability Act(HIPAA)
    - Payment Card Industry Data Security Standard(PCI DSS)

CIS 벤치마크를 구현하는 것은 규제가 심한 업종에서 비즈니스를 운영하는 조직의 규정 준수 실현을 위한 중요한 조치입니다. 잘못 구성된 IT 시스템으로 인한 규정 준수 실패 문제를 방지할 수 있습니다.

</div>
</details>
<br/>

## Session 2 : 엣지 클라우드 무결성(보안, 안정성) 검증 자동화 시스템 소개 및 데모

<details>
<summary>Click to view</summary>
<div markdown="1">
<br/>

##### 1. 엣지 클라우드 무결성(보안, 안정성) 검증 자동화 시스템 소개

각 CIS 벤치마크에는 권장 사항에 대한 설명, 권장 사항의 이유 및 시스템 관리자가 권장 사항을 올바르게 구현하기 위해 따를 수 있는 지침이 포함되어 있습니다. 
각 벤치마크는 대상 IT 시스템의 각 영역을 다루기 때문에 수백 페이지에 이를 수 있습니다. 이런 CIS 벤치마크를 구현하고 모든 버전 릴리스를 관리하는 작업은 수동으로 수행하기에 너무 복잡합니다.

이러한 이유로 엣지 클라우드 무결성(보안, 안정성) 검증 자동화 시스템을 사용하여 CIS 규정 준수를 모니터링 할 수 있습니다. 엣지 클라우드 무결성(보안, 안정성) 자동화 검증 시스템은 KaaS 지원 유형(kubeadm, k3s, microk8s)을 지원 합니다.


##### 2. 엣지 클라우드 무결성(보안, 안정성) 검증 자동화 시스템 Kaas 지원 유형별 데모

- CIS Benchmarks for kubeadm
  <br/>
  <iframe width="560" height="315" src="https://www.youtube.com/embed/hx3SB9-OTG4?si=KdxMmmRU1FRsOUAx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
  <br/>
  <br/>

- CIS Benchmarks for microk8s
  <br/>
  <iframe width="560" height="315" src="https://www.youtube.com/embed/umAp8d_rOq4?si=UH3BcW5pXMtTDuqu" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
  <br/>
  <br/>

- CIS Benchmarks for k3s
  <br/>
  <iframe width="560" height="315" src="https://www.youtube.com/embed/e9Pu6vKxIMg?si=AYviazOj9MUUMzCm" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
  <br/>
  <br/>
  
</div>
</details>

<br/>
