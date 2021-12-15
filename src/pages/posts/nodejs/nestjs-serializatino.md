---
title: NestJS Serialization
excerpt: >-
  보다 나은 코드관리를 위해 NestJS의 Request LifeCycle을 공부하는 과정에서 알게된 사실을 정리
date: "2021-12-15 19:00"
thumb_img_path: images/nestjsLogo.png
content_img_path: images/nestjsLogo.png
seo:
  title: NestJS Serialization
  description: >-
    보다 나은 코드관리를 위해 NestJS의 Request LifeCycle을 공부하는 과정에서 알게된 사실을 정리
template: post
---

Nest.js를 활용한 Bakc-End API를 개발하는 과정에서 반환값을 전달 시 어떻게 해야하는지에 대해 공부하다가 알게된 사실을 정리하였습니다.

# 직렬화(Serialization)란?

무언가를 옯기는 과정에서 프로그램이 처리하기 좋은 방향으로 변환하는 것을 의미한다. (역직렬화는 이의 반대)
나의 경험을 예시로 Client가 보내는 Request 객체를 Server가 처리하기 좋은 형태의 DTO로 변환하는 상황을 들 수 있다.

직 렬 화 : Client(json) -> (직렬화) -> Server(Object)  
역직렬화 : Server(Object) -> (역직렬화) -> Client(json)
![serialization-flow](../../../images/serialization-flow.png)

# 왜 이걸 찾아보게 되었는가?

Nest.js의 class-validator를 사용한 직렬화 방법은 알고 있었다.
Controller에서 ~~~~~~~~~~~~~~~~을 통해 Request Body의 데이터를 검증하고 파싱하는 방법으로 요청을 처리하고 있었다.
위 요청을 처리하고 반환하는 과정에서 상태, 메세지 등을 포함한 채로 전달하는 방법에 많은 고민이 생겼다.

보다 깔끔한 코드를 위해서는 Service는 단순히 객체를 반환하도록 구성하는 것이 맞고,
Contrller 단에서 Service로부터 받은 객체를 부가정보들과 함께 반환해야할 것 같다고 생각했다.  
그러나, 처음 생각한 방법으로 코드를 작성하면 너무 반복적이고, 지저분한 코드가 만들어질 것이라고 생각했다.

이를 해결하기 위해 검색해보니 Spring에는 Response Entity라는 객체를 통해 Service로부터 받은 데이터를 여러 메세지들과 함께 반환하는 것으로 보였다.  
Nest.js 자체 라이브러리 중에는 이러한 기능이 없는 것으로 보여 jojoldu님의 코드를 기반으로 구현하였다.

```js {numberLines}
// User.Controller.ts
  @Get()
  async getAllUser(): Promise<ResponseEntity<User[]>> {
    this.logger.log('User GET Request');
    return ResponseEntity.OK_WITH(await this.userService.getAll());
  }
```

```js {numberLines}
// ResponseEntity.ts
export class ResponseEntity<T> {
  @Exclude() private readonly _statusCode: string;
  @Exclude() private readonly _message: string;
  @Exclude() private readonly _data: T;

  private constructor(status: ResponseStatus, message: string, data: T) {
    this._statusCode = ResponseStatus[status];
    this._message = message;
    this._data = data;
  }

  static OK(): ResponseEntity<string> {
    return new ResponseEntity<string>(ResponseStatus.OK, '', '');
  }

  static OK_WITH<T>(data: T): ResponseEntity<T> {
    return new ResponseEntity<T>(ResponseStatus.OK, '', data);
  }

  @Expose()
  get statusCode(): string {
    return this._statusCode;
  }

  @Expose()
  get message(): string {
    return this._message;
  }

  @Expose()
  get data(): T {
    return this._data;
  }
}
```

Response Entity 구현을 통해 반복적이고 재사용이 불가능한 코드의 사용을 방지할 수 있었다.

출처 :

1. 이미지출처 : https://medium.com/@lunay0ung/basics-%EC%A7%81%EB%A0%AC%ED%99%94-serialization-%EB%9E%80-feat-java-2f3eb11e9a8
2. Nestjs 직렬화 내용출처 : https://jojoldu.tistory.com/610
