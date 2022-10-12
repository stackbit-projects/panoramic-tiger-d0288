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

이를 반영하기 위해서는 아래와 같이 사용자가 <u>**직접 sub-module을 업데이트하고 상위모듈에서 커밋하여 저장**</u>해야 한다.
```
$ git submodule update --remote
$ git commit -m "업데이트!"
```

## Sub-Tree
여러 저장소를 상위 저장소 하나가 통합해 관리하는 개념이다. Sub-Module과는 다르게 상위 저장소에 직접 파일이 추가되고 그 이력이 관리된다. Sub-Module이 링크라면 Sub-Tree는 자동동기화가 이루어지는 폴더를 복사한 것가 비슷하다.

여기서 중요한 점은 상위 저장소, 원본 저장소 모두에서 수정이 가능하며 모두 반영될 수 있다는 점이다. 이렇게 자유도가 높다는 점은 장점이면서도 단점이 된다. 자유롭게 변경이 가능하기 때문에 다른 프로젝트들을 깨뜨리는 변경이 쉽게 이루어질 수도 있기 때문이다.
```
# 메인 프로젝트 git clone
$ git clone {git addr}

# 현재 프로젝트에 subtree로 사용할 원격 저장소 추가
$ $ git remote add {repo} {repo URL}

# 원격 저장소 확인
git remote -v

# subtree 추가
# --prefix 옵션으로 서브트리를 클론할 디렉토리를 지정한다
$ git subtree add --prefix={local directory} {repo} {branch}

# 서버트리를 pull, push 할 때는 prefix 옵션을 주어서 해당 서브트리가 위치한 디렉토리 정보와
# 저장소, 브랜치를 주어서 pull, push를 수행하도록 한다.
# pull
$ git subtree pull --prefix={local directory} {repo} {branch}

# push
$ git subtree push --prefix={local directory} {repo} {branch}
```

## Spring의 멀티모듈에는?
이 글을 정리하면서 계속 들었던 생각이다.   
"Spring에 적용할 때는 어떻게 해야하지?"

물론 적용이 가능하다. Gradle의 멀티모듈을 사용해 모노레포를 구축하고 있더라도 아래의 링크와 같이 다른 모듈과의 결합이 필요한 경우가 있을 수 있다. 

[[Stack overflow] : How to include gradle multi-project repository as git submodule in another gradle project](https://stackoverflow.com/questions/57858898/how-to-include-gradle-multi-project-repository-as-git-submodule-in-another-gradl)

더 나아가서 Gradle 멀티모듈을 활용해 모노레포를 구축하고 있다면 그 범위를 "도메인"에 한정해야 하는 것 같다. 회사의 모든 프로젝트를 하나의 레포지터리에서 관리하는 것은 지양하는 방법인 것으로 보인다.


### 출처 
- [[git] git submodule / subtree](https://jammdev.tistory.com/111)
- [생황코딩 - git 강의](https://www.youtube.com/watch?v=TAe4uZqYt6c)
- [[Git] Submodule / Subtree](https://machine-geon.tistory.com/167)
