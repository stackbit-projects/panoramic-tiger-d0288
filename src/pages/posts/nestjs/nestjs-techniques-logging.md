---
title: NestJS Techniques Logging 
excerpt: >-
  NestJS Techniques Logging 문서중 궁금한 사항을 직접 구현해보면서 확인해보았습니다.
date: "2022-01-10 19:00"
thumb_img_path: images/nestjsLogo.png
content_img_path: images/nestjsLogo.png
seo:
  title: NestJS Techniques Logging 
  description: >-
    NestJS Techniques Logging 문서중 궁금한 사항을 직접 구현해보면서 확인해보았습니다.
template: post
---

# NestJS Techniques Logging 
#### (참고사항 - 개인적으로 궁금해서 공식문서 내용 확인해봄)

NestJS의 인스턴스 생성 옵션중 logger에 여러가지 옵션을 주입할 수 있다.
<mark>false</mark>, <mark>console</mark>, <mark>LoggerService(Custom Logger)</mark>  

아래의 예제 코드와 사진을 통해 확인할 수 있듯이 개발자의 입맛대로 로거를 선택/확장하여 사용할 수 있도록 만들어져 있는 것을 확인하였다.

```js
// main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    logger: console,
  });
  await app.listen(3000);
}
bootstrap();
```

![NestJS 기본 로거](../../../../images/nestjs-logger-default.png)  

![NestJS 로거 전역객체 console 커스텀](../../../../images/nestjs-logger-custom-console.png)
*Nest기본 로거가 console.log 형태로 출력되는 것을 확인할 수 있다.*

참고자료 :  
*-* [NestJS Logger Document](https://docs.nestjs.com/techniques/logger)