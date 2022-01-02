---
title: Javascript Promise
excerpt: >-
  Javascript Promise의 개념을 정리하였습니다.
date: "2021-06-24 11:31"
thumb_img_path: images/jsLogo.png
content_img_path: images/jsLogo.png
seo:
  title: Javascript Promise
  description: >-
    Javascript Promise의 개념을 정리하였습니다.
template: post
---

# Promise... Promise..? Promise..!

프로미스는 자바스크립트 비동기 처리를 위해 사용하는 객체의 이름입니다.

※ 자바스크립트 비동기 처리 : 특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 실행하는 Javascript의 특성

## Promise를 왜 쓸까요?

기존의 javascript 비동기 처리 방식에서 순차적인 로직을 처리하기 위해서 기존의 Callback Hell이 가지고 있던

에러처리의 문제를 해결하고 코드 작성에 있어서 콜백에 비해 많은 유연성, 직관성을 부여하기 때문이다.

01_Promise State (상태)
Promise는 Callback 체이닝에 비해 상태를 직관적으로 확인하여 처리하기가 용이하다.

- Pending (대기)

```js {numberLines}
new Promise(); // 기본 Promise 선언 -> 현 상태는 Pending 상태

new Promise(function (resolve, reject) {
  // callback으로 resolve와 reject를 가짐
  // resolve -> Fufilled로 Promise의 상태를 반환
  // reject -> Rejected로 Promise의 상태를 반환
});
```

- Fulfilled (이행/완료)

```js {numberLines}
new Promise(function(resolve, reject){
resolve(); // Promise 객체를 Fulfilled 상태로 반환
});

function getData(){
return new Promise(function(resolve, reject){
let data = 100;
resolve(data);
});
}

// getData 함수의 결과 값 : data(Promise, resolve) 값을 resolvedData로 받음
getData().then(function(resolvedData) {
console.log(resolvedData); // 100 출력됨
}
```

- Rejected (실패)

```js {numberLines}
new Promise(function (resolve, reject) {
  reject();
});

function getData() {
  return new Promise(function (resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값(Error)를 err 파라미터로 받음
getData()
  .then()
  .catch(function (err) {
    console.log(err); // Error: Request is failed
  });
```

![Awesome image](../../../images/promise-mozila.png)

- 여러개의 프로미스

Promise Chain에서 발생하는 모든 에러는 .catch(try-catch 아님)에서 처리된다.

(다만 Chain이 너무 많고 개별적으로 처리하려 할 시 각 각의 .then 안에서 try-catch로 처리할 수도 있다.

```js {numberLines}
function getData() {
  return new Promise({
    // ...
  });
}

// 여러 개의 프로미스를 알기 쉽게 연결하여 작성
getData()
  .then(function (data) {})
  .then(function () {
    try {
      if (res > 3) return res + 1;
      else {
        throw new Error("Error occured");
      }
    } catch (err) {
      console.log("오류 발생");
      throw err; // throw 하지 않으면 정상종료로 판단하고 다음 then이 실행됨
    }
  })
  .then(function () {
    // ...
  })
  .catch(function (error) {
    console.log(error.message);
  });
```

출처 :

https://joshua1988.github.io/web-development/javascript/promise-for-beginners/

https://coffeeandcakeandnewjeong.tistory.com/17
