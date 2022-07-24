---
title: Gradle 멀티모듈 시스템 구성 시 스프링 빈 등록 문제 
excerpt: >-

date: "2022-07-20"
thumb_img_path: images/springgradle.png
thumb_img_alt: reactLogo
content_img_path: images/springgradle.png
content_img_alt: reactLogo
seo:
  title: Gradle 멀티모듈 시스템 구성 시 스프링 빈 등록 문제
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
앞서서 멀티 모듈을 구성하는 과정에 대해 설명한 [포스팅](./gradle-multi-module-dependency-manage.md)을 작성했었습니다.  
이번에는 프로젝트를 구성하고 개발하면서 발생한 문제들에 대해서 설명해보겠습니다.

### Spring Bean 미등록
개발하면서 스프링 빈이 등록되지 않는 문제가 있었습니다.
![](images/2022-07-20-23-41-44.png)
```java
@SpringBootApplication
@EntityScan(basePackages = {"com.vrr.domain"})
@EnableJpaRepositories(basePackages = {"com.vrr.domain"})
@EnableConfigurationProperties(AppProperties.class)
public class ApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(ApiApplication.class);
    }
}
```


수정
```java
@SpringBootApplication
@EnableConfigurationProperties(AppProperties.class)
public class ApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(ApiApplication.class);
    }
}
```
![](images/2022-07-20-23-43-17.png)