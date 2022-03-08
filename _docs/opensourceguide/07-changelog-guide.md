---
layout: doc
menu: CHANGELOG 작성
title: CHANGELOG 작성 가이드
category: 오픈소스 가이드
order: 7
---
# CHANGELOG 가이드

체인지로그(changelog, 변경 기록)는 웹 사이트나 프로그램을 제작하는 것 같은 어떤 프로젝트를 진행할 때에 버그 수정이나 신규 기능 등의 변경 사항에 대한 기록입니다. 많은 오픈소스 프로젝트에서는 체인지로그 파일을 가장 상위에 포함해서 배포합니다.

체인지로그는 일종의 변경 사항에 대한 친절한 요약으로 이해할 수 있느넫, 프로젝트를 사용하는 사용자와 프로젝트에 작업중인 개발자 모두가 쉽게 이해할 수 있도록 작성되어야 합니다.

## 작성 방법
수작업으로 작성하거나 commit 메시지로부터 자동생성시키는 방법이 있습니다. 후자의 경우 CHANGELOG 작성에 대한 수고를 덜 수 있지만, 평소 commit 메시지를 정성들여 작성하지 않았을 경우에는 CHANGELOG를 이해하기 힘들 수 있습니다. 

### 작성 원칙
- 사람이 읽고 이해할 수 있도록 작성할 것
- 매 버전 마다 적어도 하나의 엔트리가 기입되어야 함
- 같은 유형의 변경은 그룹화시켜야 함
- 최근 버전을 먼저 제시해야 함
- 각 버전의 릴리스 날자를 표시해야 함

### 변경 유형
- Added: 신규 피처
- Changed: 기존 기능 변경
- Deprecated: 조만간 제거할 피처
- Removed: 삭제된 피처
- Fixed: 버그 수정
- Security: 취약점  

## 샘플
- [Sample CHANGELOG](https://gist.github.com/juampynr/4c18214a8eb554084e21d6e288a18a2c)

## 레퍼런스
- [keep a changelog](https://keepachangelog.com/en/1.0.0/)
- [What is a Changelog and How to Generate it](https://www.freecodecamp.org/news/a-beginners-guide-to-git-what-is-a-changelog-and-how-to-generate-it/)