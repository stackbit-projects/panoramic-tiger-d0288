---
title: NestJS Logging
excerpt: >-
  Nest.js에서 Winston을 활용한 Logging 서비스 구현하기
date: "2022-01-10 19:00"
thumb_img_path: images/nestjsLogo.png
content_img_path: images/nestjsLogo.png
seo:
  title: NestJS Logging
  description: >-
    Nest.js에서 Winston을 활용한 Logging 서비스 구현하기
template: post
---

# NestJS 로깅

T사 사전과제를 수행하는 과정에서 아쉽게 해결한 부분을 보완하고자 합니다.  
개발할 때와는 달리 Production을 운영/유지하는 과정에서는 문제를 확인하기 위한 로깅이 필수적인 것 같습니다. 만약 서버의 장애나 이슈가 생겼을 경우 이를 해결하기 위해 참고할 지표가 필요하기 때문입니다.

기존 ExpressJS 환경에서 개발할 때에는 <mark>morgan</mark>을 활용하여 사용자 요청에 대한 정상처리, 에러발생 등을 확인하였습니다. 하지만 이번 기회에 제가 사용하고 있던 로깅방식의 구명이 많이 뚤려있음을 깨닫고 NestJS에서는 어떻게 로깅을 처리하면 좋을 지 정리해보고자 합니다.

# NestJS 로깅 라이브러리

NestJS에는 자체 로깅 라이브러리인 <mark>Logger</mark>가 있지만, 보다 효과적이고 세세한 로그를 얻기 위해 해당 라이브러리를 확장/커스텀하여 사용해보겠습니다.

## winston

## morgan

# NestJS Custom Logger Module
