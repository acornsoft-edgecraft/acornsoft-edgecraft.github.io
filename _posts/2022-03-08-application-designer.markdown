---
layout: post
title:  Application designer
date:   2022-03-08 13:00:00 +0300
image:  application-designer01.jpeg
tags:   Design
---

Edge Cloud환경에서 다양한 엣지 서비스(5G private cloud, AL/ML, IOT, Big data, Robot, Dron, 자율 주행 등)가 `Micro service` 형태로 신속하게 배치되기 위해 필수적인 어플리케이션(`Platform application` - 통계, 모니터링, 트레이싱, CI/CD 자동화, 장애 탐지/알림, 네트워킹, 스토리지 등)을 UI상의 켄버스를 통해 쉽게 배치하고 연관된 application 사이에 미리 정의된 설정을 사용하여 다양한 형태로 디자인하고 검증하여 요구사항에 최적화된 환경을 도출하고 이를 템플릿으로 저장/로드하여 손쉽게 구성될 수 있게 한다. 

또한, 기업 엣지 어플리케이션의 입력, 출력, 통신방식(HTTP, HTTPS, TCP, GRPC 등)을 emulation하는 형태로 구성된 사용자 application을 platform application과 연동하도록 하여 Scaling, Fail-over, 성능, 모니터링, 트레이싱을 쉽게 확인하게 된다.

## Motivation
실 예로 application 로그를 수집하여 Elastic search에 저장하고 Kibana UI를 통해 확인하고자 할때 Elastic Search, Kibana, Fluentbit or Logstash의 helm chart를 모두 분석하고 상호 연관된 속성값을 정확히 설정해 주어야 한다. 즉, 보안(TLS를 위한 인증서 생성 및 설정), 연관된 Endpoint 설정, ingress 설정, application log 정규식 작성, 외부 보안 연결을 위한 cert manager를 통한 공인인증서 설정등. 이런 작업은 익숙하지 않은 경우 많은 시간과 비용이 필요하게 된다. 이와 같이 공통된 부분은 platform에서 잘 정의된 형태로 제공해 주고 사용자는 자신의 application 로그에 대한 설정에만 집중하여 즉시 연동을 시험하여 확인할 수 있어야 한다.

## Main Features

* 기업 어플리케이션 + 플랫폼 컴포넌트의 연계 및 설정에 대한 UI 프로토타입핑
* 예상된 사용자 동작에 적합한 UI 요소 및 flow 검증
* 테스트용 Mock-up 어플리케이션 속성(in/out parameter, service type 등) 설정
* 마이크로 서비스간 통신 및 데이터 확인


## Canvas concept
![]({{ site.baseurl }}/images/platform-apps-designer01.png)
*Platform Application Designer Canvas*

* 컴포넌트 layout은 활성화/비활성화 할 수 있다.
* 컴포넌트 배치는 Drag and Drop 기능으로 가능 하다.
* Group 명으로 그룹해서 공통 value 설정을 할 수 있다.
  - 클릭하면 공통 value 속성  창이 활성화 된다.
  - 배포 namespace를 지정 할 수 있다.
  - 공통 속성 값을 지정 할 수 있다.
* 배치된 컴포넌트를 오른쪽 클릭으로 속성창을 활성화 할 수 있다. (add value)
* 배치된 컴포넌트의 속성 값을 라인 연결로 전달 할 수 있다.
  - value 편집시 전달받은 속성 값을 사용 할 수 있다. (예: {{gitlab.spec.giturl}})
  - value 편집시 그룹 속성 값을 사용 할 수 있다.(예: {{group.gitops.label}})
* 배치된 컴포넌트의 속성 값이 있으면 속성값의 정보를 간략하게 화면에 노출 한다.
* 디자인을 로드/저장 할 수 있다.
* 속성 값이 완료 되면 배포 버튼으로 배포 할 수 있다.