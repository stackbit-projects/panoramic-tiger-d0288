---
title: 면접 질문 복기
excerpt: >-

date: "2022-02-11"
thumb_img_path: images/reactLogo.png
thumb_img_alt: reactLogo
content_img_path: images/reactLogo.png
content_img_alt: reactLogo
seo:
  title: 면접 질문 복기
  description: >-
    react start
  extra:
    - name: "og:type"
      value: article
      keyName: property
    - name: "og:title"
      value: React 시작하기
      keyName: property
    - name: "og:description"
      value: >-
        react start
      keyName: property
    - name: "og:image"
      value: images/reactLogo.png
      keyName: property
      relativeUrl: true
    - name: "twitter:card"
      value: summary_large_image
    - name: "twitter:title"
      value: React 시작하기
    - name: "twitter:description"
      value: >-
        react start
    - name: "twitter:image"
      value: images/reactLogo.png
      relativeUrl: true
template: post
---

# 인프런 면접 정리

## 이력서 질문

1. 현재 회사에서 하고 있는 업무는 어떤 업무이신가요?
2. 이직을 하며 회사에 아쉬웠던 점이 무엇이었을까요?
3. 입찰공고 서비스 리펙토링에서 60초 이상 걸리는 쿼리 단축은 어떤 것인가요?
4. 쿼리 성능 생선을 위해 인덱스 등을 사용해본 경험이 있나요?
5. 랜덤 쿼리를 사용할 때에 그 성능을 어떻게 최적화 하였나요?

## 과제 코드 질문

1. database connection 연결하는 비동기 과정이 어떻게 되는 것인가요?
2. WrapAsync 함수에서 async 함수만을 처리할 수 있도록 사용하고 있는데 동기 함수 또한 처리할 수 있도록 하는 방법이 있을까요?
3. 테스트 코드에서 테스트가 독립적으로 작동하지 않는데 그 이유가 무엇이며, 어떻게 해야할까요?
4. Response Entity를 사용해 코드를 작성할 때 <T>를 사용하였는데 이게 어떤 역할을 하는 것인가요?
5. Reflect가 어떤 역할을 하는 것인가요?
6. query builder를 사용해 쿼리문을 제작할 때 사용자의 실수(잘못된 메서드를 사용하는 경우)를 어떻게 방지할 수 있을까요?

## 별도 기술질문

1. 멀티 인덱싱이 무엇인지 아시나요? 어떻게 적용되고 동작하나요?
2. 분산 데이터 베이스를 운영하는 기술을 아시나요? → 아마도 샤딩?