---
title: Nodejs Logical Operator
excerpt: >-
  Javascript에서의 연산자 값을 조금더 명확히 알아본 내용을 정리했습니다.
date: '2021-11-11 23:00'
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

# 논리연산자 (Logical Operator)

or(||), and(&&)와 같은 논리연산자는 프로그래밍을 처음 배울때 변수 할당 다음으로 배웠던 기억이 있다.  
그럼 이제와서야 연산자를 "굳이" 다시 정리하는 이유는 무엇일까?

그이유는 <mark>널 병합 연산자(Nullish coalescing operator)</mark>를 알게 됨에 따라 한번쯤은 명확하게 정리하고 가야겠다는 생각이 들어서이다.

## 1. OR (||)
주변에도 물어봤었지만 OR 연산자의 결과값을 직접적으로 찍어본 분들이 많이 없는 것 같았다.  
나도 이번에 널 병합 연산자를 알게되어 찾아보던 도중 찍어보았으니 말이다.  

우선 javascript 에서의 OR 연산자는 단순히 boolean 만을 리턴하지 않는다.  
Javascript에서 OR 연산은 아래의 순서에 따라 Truthy한 값을 찾아서 리턴하는 연산이다.
1. 가장 왼쪽 피연산자부터 시작해 오른쪽으로 나아가며 피연산자를 평가합니다.
2. 각 피연산자를 불린형으로 변환합니다. 변환 후 그 값이 true이면 연산을 멈추고 해당 피연산자의 변환 전 원래 값을 반환합니다.
3. 피연산자 모두를 평가한 경우(모든 피연산자가 false로 평가되는 경우)엔 마지막 피연산자를 반환합니다.

```js {numberLines}
console.log(10 || 20) // 10
console.log(30 || 20) // 30
console.log(false || true) // true
console.log(true || false) // true
```

## 2. AND (&&)
and 연산도 OR 연산과 유사하지만 조금은 다르다.  
boolean값을 리턴하는 것이 아닌 "값"을 리턴하고 OR와는 달리 Falsy한 값을 찾아 리턴한다.
1. 가장 왼쪽 피연산자부터 시작해 오른쪽으로 나아가며 피연산자를 평가합니다.
2. 각 피연산자는 불린형으로 변환됩니다. 변환 후 값이 false이면 평가를 멈추고 해당 피연산자의 변환 전 원래 값을 반환합니다.
3. 피연산자 모두가 평가되는 경우(모든 피연산자가 true로 평가되는 경우)엔 마지막 피연산자가 반환됩니다.

```js {numberLines}
console.log(10 && 20) // 20
console.log(30 && 20) // 20
console.log(false && true) // false
console.log(true && false) // false
```

## 3. Nullish Coalescing Operator (??)
null 병합 연산자(nullish coalescing operator) ??를 사용하면 짧은 문법으로 여러 피연산자 중 그 값이 ‘확정되어있는’ 변수를 찾을 수 있습니다.  
언뜻보면 OR 연산과 유사해 보이지만 큰차이점을 가지고 있습니다.
1. <mark>||</mark> 는 첫 번째 truthy 값을 반환합니다.
2. <mark>??</mark> 는 첫 번째 정의된(defined) 값을 반환합니다.

```js {numberLines}
height = height ?? 100;
```
위와 같은 코드가 실행될 경우 height 값이 할당되어 있지 않았을 경우 height 에는 100이라는 값이 할당된다.

```js {numberLines}
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```
이러한 점에서 OR 연산과는 차이를 보이는 것이다.


## 마지막으로
끝으로 ?? 연산은 우선순위가 생각보다 낮으며, 대부분의 연산보다 나중에 연산되고 =, ?보다는 먼저 연산된다.
추가적으로 ?? 연산은 안전성 문제로 인해 &&나 || 연산과는 같이 사용이 불가하니 사용에 주의해야할 것 같다.

출처 : 
https://www.youtube.com/watch?v=dZd9tZe5w3M  
[비교하기 좋은 사이트](https://gaemi606.tistory.com/entry/NestJS-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%97%B0%EA%B2%B0-%EC%84%A4%EC%A0%95-%EC%A0%95%EB%B3%B4%EB%A5%BC-%EC%9E%85%EB%A0%A5%ED%95%98%EB%8A%94-%EB%8B%A4%EC%96%91%ED%95%9C-%EB%B0%A9%EB%B2%95-database-connection)
