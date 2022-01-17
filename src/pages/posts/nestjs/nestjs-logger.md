---
title: NestJS Logging
excerpt: >-
  Nest.js에서 Winston을 활용한 Logging 서비스 구현하기
date: "2022-01-10 19:00"
thumb_img_path: images/nestjsLogo.png
content_img_path: images/nestjsLogo.png
seo:
  title: NestJS Logging
  description: >-
    Nest.js에서 Winston을 활용한 Logging 서비스 구현하기
template: post
---

# NestJS 로깅?
로깅을 수행하는 과정에서 아쉽게 해결한 부분을 보완하고자 합니다.  
Local에서 개발할 때와는 달리 Production을 운영/유지하는 과정에서는 문제를 확인하기 위한 로깅이 필수적인 것 같습니다. 만약 서버의 장애나 이슈가 생겼을 경우 이를 해결하기 위해 참고할 지표가 필요하기 때문입니다.

기존 ExpressJS 환경에서 개발할 때에는 <mark>미들웨어</mark>을 활용하여 사용자 요청에 대한 정상처리, 에러발생 등을 확인하였습니다. 하지만 이번 기회에 제가 사용하고 있던 로깅방식의 구명이 많이 뚫려있음을 깨닫고 NestJS에서는 어떻게 로깅을 처리하면 좋을지 정리해 보고자 합니다.

# NestJS에서의 로깅전략!
winston으로 로그 레벨, 타임라인을 관리하고 미들웨어를 사용하여 요청/응답 정보들을 로깅을 할 수 있게 해서 Express 로그를 관리하는 것이 이번 로깅의 전략입니다.

# NestJS Logger 미들웨어
우선 NestJS의 미들웨어는 express의 미들웨어와 동일한 기능을 가집니다.  
라우트 핸들링 이전에 호출되며 요청(이하 Request) 및 응답(이하 Response)을 개발자 입맛에 맞게 변경합니다. 이후 <mark>next( )</mark>를 호출해 다음 미들웨어에게 전달하거나 Request-Response를 종료합니다. 
<!-- next()를 통해 정확히 어떤 값을 전달하는지 추가 필요 -->
<!-- 만약 next()를 통해 전달하지 않을 경우 Request를 계속 잡고 있다가 Timeout처리되는 것으로 알고 있으나 확인 필요 -->

NestJS의 미들웨어는 아래의 코드와 같이 NestMiddleware를 implements하여 반드시 필수 메서드 등을 구현하도록 강제합니다.

추가적으로 <mark>main.ts</mark>에서 <mark>INestApplication</mark> 인스턴스에서 제공하는 <mark>use</mark> 모든 경로를 한번에 바인딩할 수 있지만 그럴 경우 DI의 혜택을 받지 못합니다. 따라서 NestApplication 인스턴스에 <mark>app.use()</mark>를 통해 바인딩하는 행위는 지양해야 할 것 같습니다.

더불어 클래스형이 아닌 [함수형 미들웨어](https://docs.nestjs.kr/middleware#functional-middleware), [특정 라우터 제외하기](https://docs.nestjs.kr/middleware#excluding-routes), [패턴 기반으로 라우트 지정하기](https://docs.nestjs.kr/middleware#route-wildcards), [여러 미들웨어 바인딩하기](https://docs.nestjs.kr/middleware#multiple-middleware) 등의 기능을 선택적으로 활용할 수 있습니다.
```js
// logger.middleware.ts
@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  private logger = new Logger('HTTP'); // 수정요망

  use(request: Request, response: Response, next: NextFunction): void {
    const { ip, method, originalUrl } = request;
    const userAgent = request.get('user-agent') || '';
    
    // Request가 들어올 때 실행됨
    this.logger.log(`Request Detected`); 

    // Response가 종료될 때 실행됨
    response.on('finish', () => {
      const { statusCode } = response;
      const contentLength = response.get('content-length');

      this.logger.log(
        `${method} ${originalUrl} ${statusCode} ${contentLength} - ${userAgent} ${ip}`,
      );
    });

    next();
  }
}
```
```js
// app.module.ts
@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService, Logger],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(LoggerMiddleware).forRoutes('*');
  }
}
```

![asdf](../../../images/nest-middleware-log-example.png)
*LoggerMiddleware에서 로그가 정상적으로 찍히는 것을 확인할 수 있다.*

# NestJS 로깅 라이브러리
NestJS에는 자체 로깅 라이브러리인 <mark>Logger</mark>가 있지만, 보다 효과적이고 세세한 로그를 얻기 위해 해당 라이브러리를 확장/커스텀하여 사용해보겠습니다.


## winston & nest-winston
winston : 로그 파일 및 로그 레벨 관리 모듈
winston-datily-rotate-file : 매일 날짜 별로 로그 파일 생성 및 관리 모듈 ( 시간이 지나면 자동으로 삭제 & 압축 관리 )

```js
// main.ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useLogger(app.get(WINSTON_MODULE_NEST_PROVIDER));
  await app.listen(3000);
}
bootstrap();
```
*app.useLogger를 사용해 Winston Logger(Logger Service)를 전역으로 적용합니다.*

```js
// app.module.ts
@Module({
  imports: [
    WinstonModule.forRoot(getWinstonOptions('APP NAME', process.env.NODE_ENV)),
  ],
  controllers: [AppController],
  providers: [AppService, Logger],
})
```
*Module에 import하여 해당 모듈에 로거를 적용합니다.*
```js
// logger.winston.ts
export function getWinstonOptions(moduleName: string, nodeEnv) {
  const isLocalEnv = ['local', 'test', undefined].includes(nodeEnv);
  const transportsOption: ( transports.ConsoleTransportInstance | transports.FileTransportInstance )[] = 
  [
    new transports.Console({
      level: process.env.NODE_ENV === 'production' ? 'info' : 'silly',
      format: getLocalFormat(moduleName),
    }),
  ];

  if (!isLocalEnv) {
    transportsOption.push(
      new transports.File({
        dirname: 'log/error/',
        filename: 'error.log',
        level: 'error',
        format: getProductionFormat(moduleName),
      }),
      new transports.File({
        dirname: 'log/info/',
        filename: 'info.log',
        level: 'info',
        format: getProductionFormat(moduleName),
      }),
    );
  }

  return {
    transports: transportsOption,
  };
}

function getLocalFormat(moduleName: string) {
  return format.combine(
    format.timestamp({
      format: 'YYYY-MM-DD HH:mm:ss',
    }),
    nestWinstonModuleUtilities.format.nestLike(moduleName, {
      prettyPrint: true,
    }),
  );
}

function getProductionFormat(moduleName: string) {
  return format.combine(
    format.timestamp({
      format: 'YYYY-MM-DD HH:mm:ss',
    }),
    nestWinstonModuleUtilities.format.nestLike(moduleName, {
      prettyPrint: true,
    }),
    format.json(),
  );
}
```
*개발환경일 경우 로그를 콘솔로만 찍고, 배포환경일 경우 파일로 남기도록 합니다.*


#### 해당 포스트의 코드는 아래에서 확인 가능합니다.
*-* [Github Source Code](https://github.com/sysnar/nestjs-logger-workplace)  

참고자료 :  
*-* [NestJS Middleware Document](https://docs.nestjs.kr/middleware)  
*-* [NestJS Logger Middleware](https://javascript.plainenglish.io/how-to-use-nestjs-logger-2a9cb107bce9)  
*-* [Implements 설명](https://velog.io/@sgh002400/NestJS#implements-injectabledi를-알아보기)  
*-* [Morgan In Express](http://tostring.it/2014/06/23/advanced-logging-with-nodejs/)