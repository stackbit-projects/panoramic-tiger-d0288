---
title: 실시간 데이터 업데이트 로직 장애
excerpt: >-
  AJAX를 활용하여 10초간 페이지를 업데이트하는 어플리케이션의 db조회가 깨졌던 경험을 기록합니다.
date: '2021-10-28 23:21'
thumb_img_path: images/jsLogo.png
thumb_img_alt: Hikers on the trail
content_img_path: images/jsLogo.png
content_img_alt: Hikers on the trail
seo:
  title: NestJS TypeORM Settup with Confige Module
  description: >-
    NestJS의 config 파일을 코드와 분리하기 위해 몸부림친 기록
  extra:
    - name: 'og:type'
      value: article
      keyName: property
    - name: 'og:title'
      value: Basic Rules For Walking In The Mountains
      keyName: property
    - name: 'og:description'
      value: >-
        Hiking refers to difficult walking through dense forest, undergrowth, or
        bushes.
      keyName: property
    - name: 'og:image'
      value: images/jsLogo.png
      keyName: property
      relativeUrl: true
    - name: 'twitter:card'
      value: summary_large_image
    - name: 'twitter:title'
      value: Basic Rules For Walking In The Mountains
    - name: 'twitter:description'
      value: >-
        Hiking refers to difficult walking through dense forest, undergrowth, or
        bushes.
    - name: 'twitter:image'
      value: images/jsLogo.png
      relativeUrl: true
template: post
---

# 뭐가 문제였을까요..?  

문제는 역시 사람이죠...   
뭐든지 human error가 모든 에러의 대부분을 차지하는 것 같습니다.


## 그래서 뭐가 문제였는데?
서비스 실패의 문제는 hoisting으로 인한 db select query의 오염이었습니다.  
서비스 함수 안에 query를 선언할 때 let 혹은 var를 사용하여 선언하지 않고 할당하여
여러 Request가 같이 들어올 때 db의 값을 조회하는 query 변수가 가리키는 값이 변경되며
제대로된 query를 보내지 못하고 데이터값을 null을 뿜는 현상이었습니다...

## 꺼진 함수 다시보자
``` js {numberLines}
const mainFunc = () => {
  query = 'SELECT * FROM TABLE_NAME;'
  // long term db query
}

const interceptFunc = () => {
  query = 'INSTER ~~~';
  // insert & select db query
}

위와 같이 query가 함수 내에 선언되지 않을 경우 전역변수로 선언이 되게 되며
이에 따라 하나의 변수를 여러 함수가 공유하여 side effect를 야기할 수 있습니다.
```