---
title: NestJS Authentication
excerpt: >-
  Nest.js의 인증처리를 구현할 때에 사용되는 passport 모듈의 정확한 동작방식을 정리
date: "2021-12-22 23:00"
thumb_img_path: images/nestjsLogo.png
content_img_path: images/nestjsLogo.png
seo:
  title: NestJS Serialization
  description: >-
    Nest.js의 인증처리를 구현할 때에 사용되는 passport 모듈의 정확한 동작방식을 정리
template: post
---

# Nest.js에서 passport 모듈 동작방식

Nest.JS 프레임워크를 사용하며 인증을 구현하는 과정에서 인증을 디버깅하는 과정에서 어려움을 겪어 Passport 모듈 동작구조를 정리한 결과물입니다.

## 무엇이 문제인가?

우선 Passport와 Strategy를 사용해 인증을 구현할 경우 사용자 값을 제대로 인증하였는지,  
인증했다면 결과값이 어떻게 나오는지 정확하게 파악하기가 어려운 문제가 있었다.
너무나도 모듈이 추상화되어 있어서 인지 Passport Strategy를 extend하여 validate에 작성한 코드의 정상 동작여부 파악이 너무 어려웠다.

## 제작자의 말에 따르면...

Nest.JS Contributor인 "Jay McDoniel"의 말에 따르면 사용자가 @nestjs/passport의 AuthGuard를  
커스터마이징하여 사용할 경우,
해당 Guard는 "AuthGuard()#canActivate()" 메소드를 실행하고 PassportStrategy를 호출한다고 한다.
그후 PassportStrategy#validate()가 실행되며 Request 객체에 검증된 값을 포함하게 된다.

## 인증 모듈 구성전략

이번 분석을 통해 Guard#canActivate()에서는 예외를 처리하고, Strategy#validate()에서는 사용자 인증만을 처리해야겠다고 생각했습니다.
결과적으로 아래와 같은 인증 모듈을 구성하였습니다.

```js {numberLines}
// 인증 Guard 데코레이터
async canActivate(context: ExecutionContext): Promise<boolean> {
    try {
      const request = context.switchToHttp().getRequest();
      const isActivate = (await super.canActivate(context)) as boolean;

      const { user } = request.user;
      if (!user) return false;

      return isActivate;
    } catch (error) {
      this.logger.error(`JWT Auth Guard - Activate failed`, error);

      // JWT 토큰 인증에 실패할 경우 401 ERROR를 반환
      // canAcivate가 실패할 경우 error 객체에 해당 사항이 저장됨
      if (error.status === 401) {
        throw new UnauthorizedException(
          ResponseEntity.ERROR_WITH('인증에 실패하였습니다.', ResponseStatus.UNAUTHORIZED),
        );
      }

      // 이외의 모든 예외상황의 경우 500 ERROR를 반환
      throw new BadRequestException(
        ResponseEntity.ERROR_WITH('서버 문제가 발생하였습니다.', ResponseStatus.SERVER_ERROR),
      );
    }
  }
```

```js {numberLines}
// 인증 Strategy
async validate(payload) {
  const { id, role } = payload;
  const user: User = await this.userService.findById(id);

  if (Object.keys(user).length <= 0) {
    throw new UnauthorizedException('JWT Strategy - Auth Validation fail');
  }

  if (role) {
    const admin: Admin = await this.adminService.findOneByUserId(id);
    return { user, admin };
  }

  return user;
}
```

출처 :

1. 설명글 : https://stackoverflow.com/questions/65557077/passportjs-nestjs-canactivate-method-of-authguardjwt
