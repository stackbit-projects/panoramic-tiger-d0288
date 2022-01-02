---
title: Javascript Hoisting
excerpt: >-
  Javascript Hoisting의 개념을 정리하였습니다.
date: "2021-07-13 11:23"
thumb_img_path: images/jsLogo.png
content_img_path: images/jsLogo.png
seo:
  title: Javascript Hoisting
  description: >-
    Javascript Hoisting의 개념을 정리하였습니다.
template: post
---

# Hoisting

언젠가 다뤄야한다고 생각한 호이스팅(Hoisting)이라는 개념에 다뤄보고자 한다.  
Hoisting의 사전적 의미는 아래와 사진과 같이 "무언가를 끌어올린다" 이다.

![Awesome image](../../../images/hoistring_dic.png)

Javascript에서 Hoisting 은 함수, 변수(var) 등을 끌어올려 선언하기 전 상태임에도 사용할 수 있도록 하는 것이다.

기존에 C언어에서 함수를 정의하고 사용하기 위해 Code #004와 같이 사용했었던 경험이 있다.

자세하게 설명하자면 ECMA Script 5에서 변수 선언시 var 변수/함수를 스코프의 가장 위로 옮겨서 선언하는 것이다.

즉 Global 영역에 선언된 변수(const/let제외한 var만) 또는 함수는 자바사크립트 엔진이 가장 먼저 해석하게 된다

단 할당구문은 런타임과정 이루어지기 때문에 hosting이 되지 않는다.

```js {numberLines}
// Code #01
console.log(a); // undefined
var a = 100;
console.log(a); // 100
```

```js {numberLines}
// Code #002
function catName(name) {
  console.log("My cat's name is " + name);
}

catName("Tigger");
/*
위 코드의 결과는: "My cat's name is Tigger"
*/
```

```js {numberLines}
// Code #003
catName("Chloe");

function catName(name) {
  console.log("My cat's name is " + name);
}
/*
위 코드의 결과는: "My cat's name is Chloe"
*/
```

```js {numberLines}
// Code #004
#include <stdio.h>

void Hoist();

int main() {
	Hoist();

    return 0;
}

void Hoist() {
	print("Hoist Sucess");
}
```

## 호이스팅 문제점 예시)

전역변수로 생성된 a가 test 함수 내에서 재선언/초기화 되면서 첫 console.log(a)가 100이 아닌 undefined를 출력하는 것을 확인할 수 있다.

```js {numberLines}
var a = 100;

function test() {
  console.log(a); // undefined
  var a = 200;
  console.log(a); // 200
}

test();
```

위와 같은 에러를 방지하기 위해 hoisting이라는 현상을 잘 컨트롤해야 하는 필요성이 생겼다.

이를 위해서 javascript의 Scope 개념을 잘 이해해야 한다.

더 나아가 ES6에선 이를 해결하기 위해 let, const를 사용한다.

또한 'use strict'; 시 hoisting을 사용하지 못하도록 제한하기 때문에 이것 또한 좋은 방법인 것 같다.

출처 :

https://developer.mozilla.org/ko/docs/Glossary/Hoisting

https://cbw1030.tistory.com/24

https://negabaro.github.io/archive/js-Hoisting

https://pjh3749.tistory.com/201
