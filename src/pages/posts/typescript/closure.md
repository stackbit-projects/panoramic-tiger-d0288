---
title: 인터페이스에 선언되지 않는 변수값 사용
excerpt: >-
  인터페이스에 선언되지 않는 변수값 사용법을 정리
date: "2021-07-07 17:21"
thumb_img_path: images/tsLogo.png
thumb_img_alt: Hikers on the trail
content_img_path: images/tsLogo.png
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
      value: images/tsLogo.png
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
      value: images/tsLogo.png
      relativeUrl: true
template: post
---

# Typescript Class 사용법 정리

아래의 코드를 실행하는 과정에서 의문이 생겼다.

greeter 함수의 경우 Person 인터페이스를 사용한다.

즉, firstName: string / lastName: string을 사용할 수 있도록 명시한다는 소리인데

아래의 user 출력값을 보면 알듯이 제한된 2개의 변수 이외에 다른 변수들도 파라미터 값으로 넘기는 것을 알 수 있다.

예상으로는 제한된 변수 이외의 값을 넘겼으니 에러를 뿜어야하는 것이 맞다고 생각하지만

해당 코드는 TypeScript Handbook 코드이고, 에러를 내지 않았다.

검색해보니 결과적으로 정답은 다음과 같다.

"변수의 값이 인터페이스와 일치하지 않더라도, 인터페이스의 프로퍼티를 가지고 있다면 부합하는 것으로 간주한다."

추가적으로 덧붙이자면 middleInitial 값을 가지고 있더라도, 인터페이스에 존재하지 않기 때문에 tsc 컴파일시 에러를 뿜는다.

````js {numberLines}
class Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    console.log(person);
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

console.log(user);
// |-> Student {
//        firstName: 'Jane',
//        middleInitial: 'M.',
//        lastName: 'User',
//        fullName: 'Jane M. User'
//      }

console.log(greeter(user));
// |-> Hello, Jane User```
````

출처 : https://poiemaweb.com/typescript-interface
