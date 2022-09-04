---
title: Gradle7 멀티모듈 환경에서 TestFixture 공유하기
excerpt: >-

date: "2022-08-25"
thumb_img_path: images/gradleLogo.png
thumb_img_alt: reactLogo
content_img_path: images/gradleLogo.png
content_img_alt: reactLogo
seo:
  title: Gradle plugins vs build script
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
      value: images/jpaLogo.png
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


## 개요
멀티모듈 환경에서 개발하다보면 공통적인 코드를 처리하는데 어려움을 겪곤 합니다.   
이번에는 테스트 코드를 작성할 때 코드의 중복을 줄이기 위해 도메인 객체를 생성하거나, 테스트 코드 공통 설정을 처리하는 Fixture 코드가 발목을 잡았습니다. 

### 공통 Fixture를 가진 모듈
```groovy
// domain/gradle.buld

plugins {
    id 'java-library'
    id 'java-test-fixtures'
    id 'maven-publish'
}
...
```

### Fixture에 의존하는 모듈
```groovy
// domain/api/gradle.buld

dependencies {
    implementation project(':common')
    implementation project(':domain')  

    testImplementation testFixtures(project(":domain"))
    ...
}
```

출처 :   
[옛날 Gradle 설정방식](https://stackoverflow.com/questions/5644011/multi-project-test-dependencies-with-gradle)  
[Gradle7 공식문서 설정방식](https://docs.gradle.org/current/userguide/java_testing.html#sec:java_test_fixtures)
[한국어 Fixture 설정방식 설명](https://bottom-to-top.tistory.com/58)  
[테스트 의존성 관리로 높은 품질의 테스트 코드 유지하기](https://toss.tech/article/how-to-manage-test-dependency-in-gradle)