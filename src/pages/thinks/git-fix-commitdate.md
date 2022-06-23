---
title: Git Commit 일자 수정하기
excerpt: >-

date: "2022-04-06"
thumb_img_path: images/reactLogo.png
thumb_img_alt: reactLogo
content_img_path: images/reactLogo.png
content_img_alt: reactLogo
seo:
  title: 데이터베이스 인덱싱
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

```sh
$ git filter-branch --env-filter \
    'if [ $GIT_COMMIT = 90d034e3e6dc400f3a487fa3dfc33cd31f8ed96c ]
     then
         export GIT_AUTHOR_DATE="Apr 5 02:00:00 2022 +0900"
         export GIT_COMMITTER_DATE="Apr 5 02:00:00 2022 +0900"
     fi'

$ git push origin main
// 위에서 실패하면 아래명령으로 실행
git push -f origin main
```

```sh
git rebase -i ${커밋 해쉬값}
$ git commit --amend --date="Apr 5 03:00 2022 +0900"
```

https://shlee0882.tistory.com/281