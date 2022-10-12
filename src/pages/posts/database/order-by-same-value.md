---
title: ORDER BY & LIMIT (PostgreSQL)
excerpt: >-

date: "2022-10-12"
thumb_img_path: images/database-image.jpeg
thumb_img_alt: reactLogo
content_img_path: images/database-image.jpeg
content_img_alt: reactLogo
seo:
  title: ORDER BY & LIMIT (PostgreSQL)
  description: >-
    react start
  extra:
    - name: "og:type"
      value: article
      keyName: property
    - name: "og:title"
      value: 엄마가 좋아? 아빠가 좋아? (MySQL VS PostgreSQL)
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
회사에서 리더보드를 제작하면서 같은 점수를 획득한 팀이 아래의 예시와 같이 다르게 정렬되는 문제가 있었다.
문제가 발생한 쿼리는 점수를 기준으로 정렬하여 가장 높은 점수를 얻은 팀이 1위가 되도록 조회하는 쿼리였다.  
  
아래와 같이 전체 순위를 조회하는 쿼리와 상위 5개 팀만을 조회하는 쿼리의 정렬결과가 다른 것을 볼 수 있다.
```
-- SELECT * FROM leaderboard ORDER BY score desc

| id | team_name | score |     submit_at     |
|--------------------------------------------|
|  1 |  아싸가오리  |  80   |2022-09-06 06:46:19|
|  2 |  혼자왔어요  |  80   |2022-09-06 06:46:19|
|  3 |   등차수열  |   80  |2022-09-06 06:46:19|
|  4 |    ABBA   |  80   |2022-09-06 06:46:19|
|  5 |    상도덕   |  79   |2022-09-06 06:46:19|

... (+ xxx개)
```

```
-- SELECT * FROM leaderboard ORDER BY score desc LIMIT 5

| id | team_name | score |     submit_at     |
|--------------------------------------------|
|  4 |    ABBA   |  80   |2022-09-06 06:46:19|
|  1 |  아싸가오리  |  80   |2022-09-06 06:46:19|
|  3 |   등차수열  |   80  |2022-09-06 06:46:19|
|  2 |  혼자왔어요  |  80   |2022-09-06 06:46:19|
|  5 |    상도덕   |  79   |2022-09-06 06:46:19|
```

# 원인분석
이러한 문제가 발생하는 원인은 정렬하고자 하는 순서를 명확하게 제공하지 않았다는 점에 있다.   
`ORDER BY`의 기준이 되는 컬럼이 고유값을 가지지 않기 때문에 명확하게 정렬되지 않고 옵티마이저가 결정하는 최적의 방법에 따라 정렬된다.  
이때 옵티마이저는 정렬하고자 하는 row의 수 또는 LIMIT 절 존재여부 등에 맞춰 정렬을 위한 최적의 방법을 결정짓는다.  

이렇게 상황별로 정렬을 위한 방법이 변경되기 때문에 결과값이 무작위로 값이 정렬되는 것으로 보이는 것이다.

```
   사용자 : 점수를 기준으로 정렬해줘!  
옵티마이저 : 넵!   
          (진행중)  
          엇 중복되는 값이 있네...? 
          정렬하는 row 수도 적은데 빠른 A방법으로 정렬하자!
```

```
   사용자 : 점수를 기준으로 정렬하는데 결과값은 5개만 줘!  
옵티마이저 : 넵!   
          (진행중)  
          엇 중복되는 값이 있는데 ? 
          결과값 제한이 있네~ 이런 경우에는 B방법으로 정렬하자!
```
     

# 해결방법
정렬하고자 하는 컬럼에 중복값이 존재한다면 반드시 고유값을 가지는 컬럼을 `ORDER BY` 조건 마지막에 추가해주자.  
아래의 예제처럼 수정해준다면 중복값이 제멋대로 정렬되는 문제를 해결할 수 있을 것이다!

```
(기존)
SELECT * FROM leaderboard ORDER BY score desc

(수정) + submit_at 조건 추가
SELECT * FROM leaderboard ORDER BY score desc, submit_at asc
```

# 심층분석
PostgreSQL의 sort 알고리즘은 3가지가 있다. 
- Quick Sort
- Heap Sort
- Merge Sort

출처 : 
[레딧 질문글1](https://www.reddit.com/r/PostgreSQL/comments/ni2l9u/why_is_a_query_with_limit_returning_results_in_a/)  
[레딧 질문글2](https://stackoverflow.com/questions/13580826/postgresql-repeating-rows-from-limit-offset)  
[PostgreSQL 정렬 알고리즘](https://libreflare.com/9911649250298)  
[PostgreSQL limit 공식문서](https://www.postgresql.org/docs/14/queries-limit.html)