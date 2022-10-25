---
title: Spring JPA Collection 사용 시 UnsupportedOperationException 해결
excerpt: >-

date: "2022-10-25"
thumb_img_path: images/jpa-logo.png
thumb_img_alt: reactLogo
content_img_path: images/jpa-logo.png
content_img_alt: reactLogo
seo:
  title: React 시작하기
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


# 시작하면서...

최근 사이드 프로젝트를 다시 진행하면서 예전에 써두었던 잘못된 코드들이 하나둘씩 내 발목을 잡고 있다.  
악취가 나는 코드들을 수정하면서 기록해두면 나중에 많이 도움이 될 것 같아서 정리하고자 한다.

## 문제정의

1:N 양방향 관계를 가지는 Entity들이 있는 상황에서 부모(1) Entity를 `soft-delete`하기 위해 `deletedAt` 필드를 수정하고 저장하는 과정에서 `UnsupportedOperationException`이 발생하였다.  
Tour(1) <---> TourMember(N) 

일반적으로 크기가 고정인 배열(`List.of() / Arrasy.asList()`)에 `add()`와 같은 메서드를 실행할 경우 `UnsupportedOperationException`가 발생한다.
하지만 아래의 코드들에는 `List` 컬랙션 필드에 값을 추가하는 로직이 없는 것을 알 수 있다.

<br/>  

---
<br/> 

## 해결방법

문제가 될 것 같아보이는 로직이 없음에도 예외가 터진 이유는 `this.members`필드를 초기화할 때 크기가 가변적인 자료형을 사용하지 않았기 때문입니다.  
`List.of()`는 객체가 사용할 수 있는 배열의 크기가 고정되어 있어서 값 초기화시 사용을 지양하고, `new ArrayList<>()`를 통해 초기화하는 방법을 사용해야 합니다.
```java
// 수정 전
private List<TourMember> members = new ArrayList<>();

public TourGroup(TourMember leader) {
    this.members = List.of(leader); 
}
```

```java
// 해결
private List<TourMember> members = new ArrayList<>();

public TourGroup(TourMember leader) {
    this.members = new ArrayList<>(List.of(leader)); 
}
```
<br/>  

---
<br/> 

## 동작코드

![](../../../images/2022-10-25-16-48-27.png)

![](../../../images/2022-10-25-16-29-12.png)  

![](../../../images/2022-10-25-16-30-07.png)


### 연관관계

```java
@Entity
public class Tour {

    @Id
    @Column(name = "ID")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Embedded // (1) 일급 컬랙션 객체
    private TourGroup tourGroup = new TourGroup();

    ...

    @Column(name = "DELETED_AT")
    private LocalDateTime deletedAt;
}
```
- (1) 해당 연관관계는 일급 컬렉션을 JPA Entity 간 연관관계에 적용해 보고자 실험한 것입니다.
- 중요한 내용을 이해하는데에는 큰 지장이 없으니 참고해주세요.

```java
@Embeddable
public class TourGroup {

    // (2) 연관관계
    @OneToMany(mappedBy = "tour", cascade = CascadeType.PERSIST, orphanRemoval = true)
    private List<TourMember> members = new ArrayList<>();

    public TourGroup(TourMember leader) {
        this.members = List.of(leader); // (3) 이 부분이 문제가 되는 부분입니다. 아래에서 설명하도록 하겠습니다.
    }

    ...
}
```
- (2) 두 Entity는 양방향 연관관계를 맺고 있습니다.

```java
@Entity
public class TourMember {

    @Id
    @Column(name = "ID")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // (2) 연관관계
    @ManyToOne
    @JoinColumn(name = "TOUR_ID")
    private Tour tour;

    ...
}
```

<br/>  

---
<br/> 


# 심층분석
해결방법은 알았으나 해결되지 않는 궁금증이 남아 있습니다.   
위 코드에서 보면 `members`에 값을 추가하거나 빼는 코드가 존재하지 않습니다.  
그런데 왜 `UnsupportedOperationException`이 발생한 것일까요?  
<br/> 

## 콜 스택 분석
console에 찍힌 콜 스택을 통해 찬찬히 분석해보면 `PersistentBag.clear`이 호출된 뒤 `ImmutableCollections.uoe`가 실행되어 예외가 발생한 것으로 추측할 수 있습니다.
```
java.lang.UnsupportedOperationException 

at java.base/java.util.ImmutableCollections.uoe(ImmutableCollections.java:72)
at java.base/java.util.ImmutableCollections$AbstractImmutableCollection.clear(ImmutableCollections.java:78)
at org.hibernate.collection.internal.PersistentBag.clear(PersistentBag.java:495)
at org.hibernate.type.CollectionType.replaceElements(CollectionType.java:580)
at org.hibernate.type.CollectionType.replace(CollectionType.java:757)
```
<br/> 

## PersistentBag
그럼 `PersistentBag`이란 어떤 클래스이며, `clear`는 도대체 무슨 동작을 하는 메서드인지 알아보자.  

#### 클래스 설명
우선 `PersistentBag`은 Hibernate가 Entity를 영속 상태로 만들 때, 컬렉션 필드를 감싸는 Wrapper 클래스 중 하나입니다.  
`List`, `Collection` 자료형을 감싸는 Wrapper 클래스로는 PersistentBag이 사용되고   
`Set`, `List + @OrderColumn` 등을 감쌀 때는 각각 PersistentSet, PersistentList가 사용된다.  

#### PersistentBag.clear
너무 깊은 Hibernate의 실행순서는 모르기 때문에 이 부분은 추측해서 작성해두는 점 이해해주시면 감사하겠습니다.  
`save()` 메서드 실행 후 트렌젝션 종료 전 cascade 옵션에 따라(?) dirty check를 동작시키기 위해 `members`에 값이 존재하는 경우 bag.clear()와 dirty() 메서드를 실행합니다.  
이때 올바른 `List` 컬랙션 구현체라면 문제가 객체의 값을 지운 뒤, 값을 동기화하는 작업을 진행하는 것 같습니다.  
![](../../../images/2022-10-25-17-16-56.png)  

#### ImmutableCollection.ImmutableCollection.clear
그러나 저는 실수로 `List.of(leader)`를 통해 `ImmutableCollections`를 Hibernate에 제공했습니다.  
그리고 `ImmutableCollections`는 아래와 같이 clear 메서드를 구현하지 않고 예외를 던지기 때문에 `UnsupportedOperationException`가 발생하게 된 것 입니다.

비슷한 내용으로 [관련된 이전 포스팅](https://sysnar.github.io/posts/spring/java-unsupported-operation-exception/)에서도 다루었습니다.
![](../../../images/2022-10-25-17-51-43.png)

<br/> 

## Hibernate 프로젝트 논의

마지막으로 이러한 문제에 대해서 다른 사람들의 생각을 찾아보다가 아래의 내용을 찾게 되었습니다.  
  
또한 Hibernate 프로젝트에서 논의한 바로는 `bag.clear()`가 특정 상황에서는 불필요하게 실행되고 있다는 것 같다.  
사실상 Hibernate를 제대로 사용했다면 발생하지 않는 문제여서 사실상 수정 우선순위는 낮은 것으로 보인다.  

![](../../../images/2022-10-25-17-34-28.png)
링크 : https://hibernate.atlassian.net/browse/HHH-13349
<br/>  

---
<br/> 

#### 출처 :   
\* [[자바 ORM 표준 JPA 프로그래밍] 14.1 컬렉션](https://milenote.tistory.com/148)

\* [Java - ArrayList 초기화, 4가지 방법](https://codechacha.com/ko/java-collections-arraylist-initialization/)

\* [Java - Arrays.asList vs List.of 차이 (완벽 정리)!](https://jaehoney.tistory.com/144)
