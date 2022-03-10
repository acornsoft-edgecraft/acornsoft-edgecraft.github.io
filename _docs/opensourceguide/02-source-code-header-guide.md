---
layout: doc
menu: 소스 코드 주석 작성
title: 소스 코드 헤더 주석 작성 가이드
category: 오픈소스 가이드
order: 2
---
# 소스 코드 헤더 주석 작성 가이드

소스 코드 시작 부분에 저작권과 리이선스 정보를 주석으로 명시해야 합니다. 다음 두 가지 경우 모두 해당합니다.
- 소스 코드 파일을 직접 생성한 경우
- 외부 오픈소스 프로젝트 파일을 가져와 수정해서 사용한 경우

## 소스코드 저작권 
소스 코드 저작권 고지는 일반적으로 "copyright"라는 단어를 포함하여 연도와 저작권자 혹은 회사명을 포함하는 문장열로 이루어 집니다. 오픈소스 라이선스에 따라 특허가 있는 경우라면, 특허 소유자와 오픈소스 기여자들을 표시하기도 합니다. 연도는 일반적으로 최초 게시된 연도를 의미하며, 저작권 보호기간을 결정하는데 사용될 수 있도록 저작권 기간을 의미하기도 합니다.

## 소스 코드 헤더 주석 작성 예시

### 네이버 [Pinpoint](https://github.com/pinpoint-apm/pinpoint/blob/90c7603681a3fc310faead9ad47cc358ad49c04e/web/src/main/webapp/components/jquery-ui/ui/jquery.ui.datepicker.js) 프로젝트 경우

```
/*!
 * jQuery UI Datepicker 1.10.4
 * http://jqueryui.com
 *
 * Copyright 2014 jQuery Foundation and other contributors
 * Released under the MIT license.
 * http://jquery.org/license
 *
 * http://api.jqueryui.com/datepicker/
 *
 * Depends:
 *	jquery.ui.core.js
 */
```
<br/>

### 네이버 [egjs](https://github.com/naver/egjs-rotate/blob/4c8711fd6e7b9f5592d3c872270f64b4aefb6763/config/validate-commit-msg.js) 프로젝트 경우

이 경우는 Angular JS 프로젝트의 특정 파일을 수정해서 사용하였음을 명시하고 있습니다.

```
#!/usr/bin/env node

/**
 * Original Code
 * https://github.com/angular/angular.js/blob/v1.5.9/validate-commit-msg.js
 * Git COMMIT-MSG hook for validating commit message
 * See https://github.com/naver/egjs/wiki/Commit-Log-Guidelines
 * modified by egjs
 */
```