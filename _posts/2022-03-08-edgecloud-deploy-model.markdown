---
layout: post
title:  Edge Cloud 배포 모델
date:   2022-03-08 10:01:35 +0300
image:  deploy-model01.png
tags:   Design
---

다수의 Edge cloud상의 cluster를 통합관리하기 위해 각 Edge cloud에 관리 클러스터(Management Cluster)를 설치하고 Edge cluster 설치, 노드 확장, 업그레이드, 삭제등의 작업을 CR(Custom Resource)를 활용하여 Cluster controller가 제어하도록 한다. VM을 이용한 edge cluster 구성시 우선 openstack cluster를 kubernetes상에 배포(openstack controller, CR 활용)한 후 cluster controller로 edge cluster를 구성하게 된다.

Management cluster에서는 edge cluster에 설치된 prometheus가 전송하는 montoring metric을 Promscale을 통해 수집하며 Grafana를 통해 Edge Cloud상의 cluster를 모니터링 한다. Workload배포는 오픈 소스로 제공되는 kubesphere, kore-board, kubernetes dashboard 등을 활용할 수 있게 한다.

## 배포 모델 고려사항

* Edge cloud내 클러스터의 생성/업그레이드/확장은 Central cloud에서 제공하는 WEB, cli를 통해 각 Edge Cloud의 클러스터를 관리하기 위한 관리 클러스터를 통해 구성한다.
* Management cluster는 엣지 클라우드 하위 다수의 엣지 클러스터의 라이프사이클(CRD, controller를 통해) 관리, 모니터링 메트릭 정보를 수집함.
* Edge cluster에서 발생하는 알람, 이벤트 정보는  Management cluster의 Agent를 통해 Central Cloud로 전달됨.
* Edge cluster는 baremetal type과 openstack 상의 VM type으로 구성될 수 있음.
* Baremetal type과 VM type을 혼재하여 edge cloud를 사용할 수 있다. 예를 들어, baremetal 장비에 GPU를 탑재하여 고도의 계산이 필요한 AI/ML application을 배치하고 VM에는 통신용 application을 배치하여 사용할 수 있다.
* 엣지 서비스를 위해 플랫폼에서 제공되는 어플리케이션은 디자이너를 통해 템플릿화하여 배포할 수 있다.
* 각 엣지 클러스터에서 사용되는 Storage는 ceph storage를 별도로 구성하거나 기업내 스토리지(NAS, ceph)등을 활용할 수 있다.
* Baremetal node가 IPMI(Intelligent Platform Management Interface) 기능을 지원하는 메인보드일 경우 Central Cloud를 통해 전원 on/off 및 OS 재설치를 할 수 있다.
* Central cloud와 edge cloud간 보안 통신을 위해 Edge controller <-> Agent 간 VPN 으로 통신한다.


![]({{ site.baseurl }}/images/deploy-model02.png)
*엣지 클라우드 배포 모델*