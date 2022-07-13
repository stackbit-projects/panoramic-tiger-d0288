---
title: @RequestParam 데이터를 Setter 없이 DTO에 매핑하기
excerpt: >-

date: "2022-06-21"
thumb_img_path: images/architecture-thumbnail.png
thumb_img_alt: reactLogo
content_img_path: images/architecture-thumbnail.png
content_img_alt: reactLogo
seo:
  title: 도메인 아키텍처
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
      value: images/reactLogo.png
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

조회관련 기능을 개발하면서 꿀팁이 생겨서 기록하고자 한다.
@RequestParam 데이터를 바인딩할 때 DTO를 사용하고 싶어서 적용하는 과정에서 아래와 같은 궁금증이 생겼다.
1. QueryString 값을 DTO로 매핑이 가능한지?
2. DTO로 매핑한다면 그 값을 불변하게 관리할 수 있는지?

```java 
@Getter
public class DTO {

    @NotEmpty
    private String id;

    @NotEmpty
    private String storage;

    @JsonCreator
    public DTO(@JsonProperty("id") String id,
               @JsonProperty("storage") String storage) {
        this.id = id;
        this.storage = storage;
    }
}
```

출처 : [다양한 필드 매핑 방법에 대한 논의](https://stackoverflow.com/questions/40896217/how-to-bind-request-params-without-setters)