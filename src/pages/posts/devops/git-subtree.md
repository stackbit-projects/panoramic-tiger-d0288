---
title: Git Sub Tree / Sub Module
excerpt: >-

date: "2022-10-05"
thumb_img_path: images/gitLogo.png
thumb_img_alt: gitLogo
content_img_path: images/gitLogo.png
content_img_alt: gitLogo
seo:
  title: Git Sub Tree / Sub Module
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


# 개요
블로그에 필요한 예시를 저장하기 위한 [저장소](https://github.com/sysnar/blog-codes)를 구축하다보니 git의 sub-module / sub-tree 라는 개념을 알게 되어 정리하고자 한다.

## Sub-Module 
git 저장소 안에 다른 여러 저장소를 만들어 관리하는 개념이다. 각각 존재하는 저장소를 **리눅스의 심볼릭 링크 연결하듯이 각 모듈의 저장소 위치를 저장함**으로써 느슨하게 연결해둔 관리 방법이다.  
각 모듈의 저장소 위치는 `.gitmodules` 라는 파일을 통해 관리된다.  
여기서 중요한 점은 모든 모듈이 하나의 저장소로써 취급되기 때문에 하위 모듈의 변경사항이 자동으로 상위 모듈에 반영이 되지 않는다는 점이다.
아래와 같이 super 모듈 내부에 있는 sub1 모둘에서 변경사항이 발생하더라도 super 모듈에는 변경사항이 알아서 반영되지 않는다.

```
super
|-- sub1 
|    |- 2.txt // 1.txt에서 2.txt로 수정됨
|-- sub2
```

이를 반영하기 위해서는 아래와 같이 사용자가 직접 sub-module을 업데이트하고 상위모듈에서 커밋하여 저장해야 한다.
```
$ git submodule update --remote
$ git commit -m "업데이트!"
```

## Sub-Tree
Sub-Module과는 다르게 여러 저장소를 상위 저장소 하나가 통합해 관리하는 개념이다.

## Spring의 멀티모듈에 적용할 때는?

### 출처 
- [[git] git submodule / subtree](https://jammdev.tistory.com/111)
- [생황코딩 - git 강의](https://www.youtube.com/watch?v=TAe4uZqYt6c)