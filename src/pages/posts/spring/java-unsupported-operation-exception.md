---
title: Java - UnsupportedOperationException
excerpt: >-

date: "2022-09-27"
thumb_img_path: images/javaLogo.png
thumb_img_alt: javaLogo
content_img_path: images/javaLogo.png
content_img_alt: javaLogo
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

# 개요
프로젝트를 하면서 `Arrays.asList()`를 사용해 생성한 리스트에 값을 추가할 수 없는 문제가 있었다.  
그냥 넘어갈 수도 있었지만 Java의 Collection Framework에 대해 깊이 공부해본 적이 없기도 하고 동작원리가 궁금해서 찾아보게 되었다.

<br>

## 요약
1. `Arrays.asList()`, `List.of()`는 크기를 변경할 수 없다. 테스트 코드에 용이하다.
2. `Arrays.asList()`는 null 값을 저장할 수 있지만, `List.of()`는 저장할 수 없다.
3. `new ArrayList<>()`의 경우 원본 배열을 새로운 메모리 주소값에 할당하는 깊은 복사를 사용한다.

<br>

## 문제
문제를 맞닥뜨렸을 때의 생각은 이랬다. `List.of()`, `Arrays.asList()`, `new ArrayList()` 모두 List를 반환하는데 왜 ArrayList만 유동적인 갖지?  

<br>

## 문제 정의
조금 생각해보니 List는 그저 Interface일 뿐이기 때문에 이를 구현하는 구현체 간의 차이점 때문일 것이라고 생각했다.  
그럼에도 확실하게 "이것 때문이다!"라는 답을 얻을 수 없어서 구현체 간의 차이점을 직접 찾아봄으로써 해결하고자 했다.
그러기 위해 우선 각 경우에 대한 테스트 코드를 통해 명확히 문제를 정의했다.

테스트 코드는 [Github 코드 저장소](https://github.com/sysnar/blog-codes/blob/main/java/src/test/java/arraylist/ArrayListTest.java)에서 돌려볼 수 있습니다.


<br>

## Array
JavaScript와는 다르게 Java의 배열은 사이즈가 고정적이다.  
그렇기 때문에 아래와 같이 명시적으로 크기가 고정된 배열에 값을 추가하려고 하면 컴파일 에러가 발생한다.
```java
Integer[] integers = {1, 2, 3};
// integers.add(4); (컴파일 에러)
```

```java
@Test
void Array_add_Should_throw_error() {
    // given
    Integer[] integers = {1, 2, 3};

    // when & then
    // integers.add(4);
    // 위와 같은 경우에는 컴파일 에러가 발생한다.
    assertThat(integers[1]).isEqualTo(2);
    assertThrows(
            ArrayIndexOutOfBoundsException.class,
            () -> {
                int noneExistElement = integers[3];
            }
    );
}
```
이렇게 고정적인 배열의 단점을 극복하기 위해 Java는 `Collection Framework`를 지원한다.
List, Map, Set 등의 자료구조에 대한 Interface를 정의하고 있다. 이중 List 자료구조를 구현하는 구현체 중 하나인 `ArrayList`에 대해서 더 깊게 알아보고자 한다.

<br>

## Arrays.asList()
`Arrays.asList()` 매서드의 내부 구현을 보면 ArrayList 클래스를 return하고 있는 것을 알 수 있다. new ArrayList()와 동일한 타입인데 동작은 다르다니 말이 되지 않는다고 생각할 수 있지만 이것에는 함정이 있다.
![](../../../images/2022-09-27-22-54-58.png)
![](../../../images/2022-09-27-22-59-39.png)
위 사진을 보면 Arrays 클래스에는 ArrayList라는 Inner Class가 존재한다. 앞서 혼란을 주었던 `Arrays.asList()` 매서드가 return하고 있는 ArrayList의 정체가 바로 이녀석인 것이다.

그렇다면 이제 아래와 같이 배열에 원소를 추가하려할 때 예외가 발생하는 이유에 대해 알아보자.
원인은 바로 Inner Class인 ArrayList가 상속하고 있는 `AbstractList`에 있다.
```java
@Test
void Arrays_asList_add_Should_throw_error() {
    // given
    List<Integer> integers = Arrays.asList(1, 2, 3, 4);

    // when & then
    assertThrows(
            UnsupportedOperationException.class,
            () -> integers.add(5)
    );
}

```

AbstractList 클래스는 List Interface가 제공해야할 기본적인 메서드들을 구현하고 있다. 이때 구현된 메서드는 아래와 같이 `UnsupportedOperationException` 예외를 발생시키고 있다. 따라서 AbstractList를 상속하고 구현하지 않은 **Inner Class: ArrayList** 클래스는 add와 같은 기본적인 List 인터페이스 메서드를 실행하면 예외가 터지게 되는 것이다.
![](../../../images/2022-09-27-23-05-34.png)

<br>

## List.of()
그렇다면 `List.of()` 메서드의 경우에는 아래 테스트 코드와 같이 문제가 발생하는 것일까?  

```java
@Test
void List_add_Should_throw_error() {
    // given
    List<Integer> integers = List.of(1, 2, 3, 4);

    // when & then
    assertThrows(
            UnsupportedOperationException.class,
            () -> integers.add(5)
    );
}
```
그 이유는 `Arrays.asList()`의 경우와 같다. `List.of()` 메서드도 ArrayList 클래스와 같이 `UnsupportedOperationException`을 던지는 기본 클래스를 반환하고 있기 때문이다. `List.of()` 메서드는 파라미터 개수에 따라 List12 또는 ListN이라는 클래스를 생성하는데 이들 모두가 `AbstractImmutableList`클래스를 상속받고 있다.
![](../../../images/2022-09-27-23-13-52.png)


<br>

## new ArrayList()
그럼 유연하게 원소를 추가할 수 있었던 ArrayList 클래스는 과연 어떻게 아래 테스트코드와 같이 동작하고 있는지 궁금해지기 마련이다. 
 
```java
@Test
void ArrayList_add_Should_be_added() {
    // given
    List<Integer> integers = new ArrayList<>();
    integers.add(1);
    integers.add(2);
    integers.add(3);
    integers.add(4);

    // when
    integers.add(5);

    //  then
    assertThat(integers.get(4)).isEqualTo(5);
}
```

ArrayList는 앞선 두 클래스들과는 다르게 add() 메서드를 구현하고 있다. 이 과정에서 앞선 두 클래스들과는 다른 로직이 있는데, 클래스 내부에 배열을 관리하는 필드가 존재하고 값을 추가할 때마다 이 배열의 크기를 늘려준다는 것이다.  
이 덕분에 앞선 두 클래스들과는 다르게 자료의 크리를 조절할 수 있는 것으로 보인다.
```java
transient Object[] elementData; // non-private to simplify nested class access

// ...

private Object[] grow(int minCapacity) {
    return elementData = Arrays.copyOf(elementData,
                                        newCapacity(minCapacity));
}
```

# 정리 
사용만 하던 기능들의 실제 동작원리를 구현 레벨에서 분석해가며 확인해보니, 문제가 될 수 있는 지점을 정확하게 알고 사용할 수 있게 되었다.  
역시 기술, 기능의 단순 사용법 보다는 그 원론적인 이유, 원리에 입각해 공부하는 것이 중요함을 다시 한번 깨닫게 되는 순간이었다.

출처 : [new ArrayList<>()와 Arrays.asList()와 List.of()](https://kim-jong-hyun.tistory.com/31)  
