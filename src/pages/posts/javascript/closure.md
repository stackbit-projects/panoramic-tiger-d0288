---
title: Javascript Closure
excerpt: >-
  Javascript Closure 개념을 다시한번 정리하였습니다.
date: "2021-10-06 24:00"
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
    - name: "og:type"
      value: article
      keyName: property
    - name: "og:title"
      value: Basic Rules For Walking In The Mountains
      keyName: property
    - name: "og:description"
      value: >-
        Hiking refers to difficult walking through dense forest, undergrowth, or
        bushes.
      keyName: property
    - name: "og:image"
      value: images/jsLogo.png
      keyName: property
      relativeUrl: true
    - name: "twitter:card"
      value: summary_large_image
    - name: "twitter:title"
      value: Basic Rules For Walking In The Mountains
    - name: "twitter:description"
      value: >-
        Hiking refers to difficult walking through dense forest, undergrowth, or
        bushes.
    - name: "twitter:image"
      value: images/jsLogo.png
      relativeUrl: true
template: post
---

# Javascript 클로저(Closure)

1. [실행 컨텍스트 (Execution Context)](https://sysnar.github.io/posts/javascript/scope/)
2. [클로저](https://sysnar.github.io/posts/javascript/closure/)
3. [lexical scoping](https://sysnar.github.io/posts/javascript/lexical%20scoping/)

## 클로저(Closure)의 정의

클로저란 특정한 함수를 의미하며, 스코프의 개념을 이용하여 변수에 접근할 수 있는 범위(scope)를 제한하는 개념입니다.

따라서 클로저 함수의 특징을 다음과 같이 설명할 수 있습니다.

- 외부함수에서 내부함수 스코프에 접근할 수 없다.
- 내부 함수에는 외부 함수 스코프에 접근할 수 있다.
- 외부 함수의 실행이 종료된 후에도 클로저 함수는 외부 함수 스코프에 접근할 수 있다.

이 내용과 관련된 좋은 코드가 `제로초(조현영)`님의 블로그에 있어 첨부합니다.

```js {numberLines}
let makeClosure = function () {
  let name = "zero";
  return function () {
    console.log(name);
  };
};
let closure = makeClosure(); // function () { console.log(name); }
closure(); // 'zero';
```

위의 코드처럼 전역 스코프에서는 name 변수에 접근할 수 없지만,  
makeClosure 내부 함수에서는 접근하여 값을 출력하는 것을 확인할 수 있습니다.

## 클로저(Closure)를 쓰는 이유

1. 데이터 보존 : 함수의 실행이 끝나더라도 데이터를 스코프 안에 간직한 채로 계속 사용할 수 있습니다.
2. 캡슐화 : 클로저 함수에 값이 종속되어 외부에서 접근할 수 없기 때문에 안전하게 데이터의 불변성을 지킬 수 있습니다.

## 결과적으로 클로저(Closure)란

함수를 리턴하는 함수가 아니라 내부함수(function)가 상위 스코프(makeClosure)의 값을 참조하고 있고 외부에서 실행하였을 때  
상위 스코프의 값(name)을 수정할 수 없는 형태를 의미합니다.

출처 :  
https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0  
https://hanamon.kr/javascript-%ED%81%B4%EB%A1%9C%EC%A0%80/
