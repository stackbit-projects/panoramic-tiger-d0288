---
title: Javascript Closure
excerpt: >-
  Hiking can sometimes involves bushwhacking and hiking is sometimes referred to
  as such. This specifically refers to difficult walking through dense forest,
  undergrowth, or bushes, where forward progress requires pushing vegetation
  aside.
date: '2021-10-06 24:00'
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

# Javascript 실행컨텍스트(Execution Context)

javascript 에서의 this를 공부하기 전 scope의 개념을 정확하게 설명할 수 있도록 정리하려고 합니다.

1. 실행 컨텍스트 (Execution Context)
2. 클로저
3. lexical scoping

## Execution Context?
우선 `Execution Context`라는 단어의 뜻은 아래와 같다.  
실행 문맥 : javascript가 실행되는 문법의 흐름
   
```js {numberLines}
var name = 'zero'; // (1)변수 선언 (6)변수 대입

function wow(word) { // (2)변수 선언 (3)변수 대입
  console.log(word + ' ' + name); // (11)
}

function say () { // (4)변수 선언 (5)변수 대입
  var name = 'nero'; // (8)
  console.log(name); // (9)
  wow('hello'); // (10)
}

say(); // (7)
```

출처 : https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0