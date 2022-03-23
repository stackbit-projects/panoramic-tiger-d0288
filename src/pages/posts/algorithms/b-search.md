---
title: Binary Search  
excerpt: >-
  Leetcode에서 Binary Search를 문제를 해결하는 과정과 그 해답입니다.
date: '2022-03-23 21:00'
thumb_img_path: images/jsLogo.png
thumb_img_alt: Hikers on the trail
content_img_path: images/jsLogo.png
content_img_alt: Hikers on the trail
seo:
  title: Binary Search  
  description: >-
    Leetcode에서 Binary Search를 문제를 해결하는 과정과 그 해답입니다.
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

# 이진탐색(Binary Search)

이진탐색이란 주어진 배열을 반씩 쪼개가며 찾고자 하는 값을 찾을 때까지 탐색하는 알고리즘이다.  
그렇기 때문에 이진탐색 알고리즘을 적용하기 위해선 배열이 반드시 정렬되어 있어야 한다.  
만약 0 ~ 6이 들어있는 배열에서 5를 찾고자 한다면 아래와 같은 방식으로 검색하게 된다.  

### 첫번째 탐색
 [0, 1, 2, <mark>3</mark>, 4, 5, 6]  

  우선 배열의 전체 길이를 반으로 나눈 값을 내립하여 가장 가운데에 존재하는 값(아래부턴 편의상 pivot이라고 함)을 찾는다.  
  그다음 pivot이 목표값과 같은지 비교하고 같을 경우 반환, 다를 경우 계속해서 검색한다.  
  목표값이 pivot보다 크기 때문에 [**pivot + 1** ~ **배열의 끝**] 까지 검색한다.   

### 두번째 탐색
2. [4, <mark>5</mark>, 6]  
  pivot이 목표값과 동일하다. 검색을 종료한 뒤 검색의 결과값으로 <mark>5</mark>를 반환한다.

### 종료 조건
이진탐색 알고리즘의 종료조건은 크게 2가지이다.
1. 검색과정에서 목표값을 찾는 경우
2. 검색과정 배열의 크기를 1까지 줄여가며 탐색하였으나, 목표값을 찾지 못한 경우
  
<hr/>  

### 시간복잡도
결과적으로 N개의 배열을 검색할 때에 N, N/2, N/4, N/8, ..., 1으로 탐색할 자료의 수가 줄어들기 때문에 
시간 복잡도는 Big O표기법으로 O(logN)으로 나타낼 수 있습니다.


<mark>그럼 여기서 한가지 의문을 가지게 됩니다.</mark>   
아래의 예제코드를 보면 반복문을 돌리는 과정에서 배열을 0부터 끝까지 반복하지 않는데   
과연 N, N/2 ... 의 시간이 실제로 소요되느지 의문이 생겼습니다.

의문에 대한 답은 이렇습니다.   
제가 생각한 방식은 선형적으로 검색을 했을 때의 방식이었습니다.  
이진 탐색은 선형적으로 동작하지 않기 때문에 시간 복잡도의 측정 또한 선형적인 방식들과는 달랐습니다.  

공부를 통해 배운 핵심은 이진탐색은 탐색 연산을 수행하고 탐색할 데이터를 반으로 줄여나가는 알고리즘이라는 것입니다.  
정리하자면 목표값을 찾고 탐색할 데이터를 나누는 연산이 일어나는데, 그게 최약의 경우 1이 될 때까지 일어나기 때문에 아래와 같은 수식을 가지게 됩니다.  
<mark>N * (1/2)^k = 1</mark> (k값은 탐색횟수)   


### 예제코드 - 반복문
```javascript
function Search(nums, target) {
  let left = 0;
  let right = nums.length - 1;

  while (left <= right) {
    let pivot = Math.floor((left + right) / 2);

    if (nums[pivot] == target) {
      return pivot;
    } else if (nums[pivot] > target) {
      right = pivot - 1;
    } else {
      left = pivot + 1;
    }
  }

  return -1;
}
```