---
title: Javascript Lexical Scoping
excerpt: >-
  Hiking can sometimes involves bushwhacking and hiking is sometimes referred to
  as such. This specifically refers to difficult walking through dense forest,
  undergrowth, or bushes, where forward progress requires pushing vegetation
  aside.
date: '2021-10-07 00:20'
thumb_img_path: images/jsLogo.png
thumb_img_alt: Hikers on the trail
content_img_path: images/jsLogo.png
content_img_alt: Hikers on the trail
seo:
  title: Javascript Gatter와 Setter
  description: >-
    Hiking refers to difficult walking through dense forest, undergrowth, or
    bushes.
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

# Javascript Lexical Scoping

javascript 에서의 this를 공부하기 전 scope의 개념을 정확하게 설명할 수 있도록 정리하려고 합니다.

1. [실행 컨텍스트 (Execution Context)](https://sysnar.github.io/posts/nodejs/scope/)
2. [클로저](https://sysnar.github.io/posts/nodejs/closure/)
3. [lexical scoping](https://sysnar.github.io/posts/nodejs/lexical%20scoping/)

## Scope란?

javascript를 쓰다보면 `Scope`라는 단어를 참 많이도 접하지만, 정작 한줄로 설명하기는 어려운 것 같습니다.

`Scope`는 한마디로 <mark>변수에 접근할 수 있는 범위</mark>라고 정의할 수 있습니다.

이런 Scope에는 2가지 종류가 있습니다.
1. 전역 스코프(Global Scope)
2. 지역 스코드(Local Scope)

Global Scope는 웹 브라우저의 경우(window, docuement), Node.js의 경우(global) 등의 객체에 선언되어 어느 곳이던지 해당 변수에 접근하여 사용할 수 있다는 뜻이며,  
Local Scope는 특정 범위가 주어지고 그 범위 안에서만 접근할 수 있다는 것을 의미합니다.

이 내용을 이해하기 위한 간단한 예시가 있습니다.
```js {numberLines}
let x = 'global';
function ex() {
  x = 'change';
}
ex();
alert(x); // 'change'
```
  
javascript의 특성상 변수의 범위를 호출한 함수의 Local Scope부터 전역 변수들이 있는 
Global Scope까지 범위를 점차 넓혀가며 찾기 때문에 함수 ex의 Scope 내에 없는 변수 x를 찾아 값을 변경하게 됩니다.  
만약 함수 ex 내에 변수 x가 존재했다면 지역변수 x에 값을 할당하고 전역변수의 값은 변경되지 않았을 것 입니다.  
...  
![Awesome image](scopeExample.png)  
...  

## Scope Chain  


## Lexical Scoping  

출처 :   
https://www.zerocho.com/category/Javascript/post/5740531574288ebc5f2ba97e  
https://medium.com/@su_bak/javascript-스코프-scope-란-bc761cba1023