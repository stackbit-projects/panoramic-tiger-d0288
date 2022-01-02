---
title: React 시작하기
excerpt: >-

date: "2021-06-09"
thumb_img_path: images/reactLogo.png
thumb_img_alt: reactLogo
content_img_path: images/reactLogo.png
content_img_alt: reactLogo
seo:
  title: React 시작하기
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

![Awesome image](../../../images/react-event-handler.png)

# React 시작하기

React에서 onClick과 같은 이벤트를 핸들링할 수 있는 이벤트 핸들러에 매개변수를 전달할 때 아래의 규칙을 따르는 것 같다.

```js
functionName(...params, e[Event객체 혹은 this])
```

공식 문서에서 이렇게 사용하라고 되어 있으나 e, ...params와 같이 사용할 수 없는 이유에 대해서는 아직 의문이다.

이부분은 추가적으로 구글링하여 업데이트할 계획이다.

출처 : https://ko.reactjs.org/docs/handling-events.html
