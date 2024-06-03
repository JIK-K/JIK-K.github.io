---
layout: post
title: " [NestJS]- NestJs Middleware"
categories: NestJS
tags: [NestJS, NodeJS, Typescript, MiddleWare]
---

## Middleware

<span style = "color:#FF8C00">Middleware</span>은 라우터 핸들러 이전에 호출되는 함수이다.

Middleware 함수는 요청 및 응답 개체와 애플리케이션의 요청 응답 주기에 있는 미들웨어 함수에 접근 할수 있고, 일반적으로 미들웨어 함수는 next라는 변수로 표시된다.
Nest Middleware은 기본적으로 Express 미들웨어와 동일하다.

즉, Middleware는 http 요청과 응답사이 라우터 핸들러 이전에 호출되는 함수이며, 클라이언트 요청을 라우터 핸들러가 받기 전에 가로채서 다른 작업을 수행할 수 있게 한다.

![NestJs](/assets/images/Middlewares_1.png)
<small>출처:<https://docs.nestjs.com/middleware></small>

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">Middleware 특징</span><br/>

- request에 대한 검증, 로깅, 인증, 권한 체크, 캐싱 등의 작업을 수행할 수 있다.
- 다양한 작업을 순차적으로 처리할 수있다.
- next 함수 호출을 통해 middleware 체인을 연결한다. next 함수 호출을 통해 다음 middleware가 실행되며 호출하지 않으면 다음 middleware가 실행되지 않는다.
- NestJs는 전역 미들웨어(Global Middleware)와 로컬 미들웨어(Local Middleware)을 지원하며, 전역 미들웨어는 모든 요청과 응답에 적용되고, 로컬 미들웨어는 특정 라우터에만 적용된다.

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">Middleware 함수가 수행할 수 있는 작업</span><br/>

- 코드를 실행한다.
- request, response응답 개체를 변경한다.
- requset-response 응답 사이클을 종료한다.
- stack에서 다음 middleware 함수를 호출한다.
- 현재 middleware 함수가 request-response 사이클을 종료하지 않는 경우 다음 middleware 함수에 제어권을 전달하기 위해서 호출되어야 한다. 그렇지 않으면 request가 중단된 상태로 남게된다.

### MiddleWare 생성

```typescript
@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log("Request...");
    next();
  }
}
```

use 메서드를 이용하여 Middleware를 구현하는데 매개변수로 req, res, next를 받는다.
req는 요청객체, res는 응답객체, next는 다음 미들웨어를 호출하는 함수이다.

### MiddleWare 적용

```typescript
@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
      .forRoutes('*'); //전역에 등록
    //   .forRoutes({ path: 'cats', method: RequestMethod.GET }); //특정 경로 등록
    //   .forRoutes(CatsController) //Controller Class도 연결 할 수 있다.
      .exclude(
        // { path: 'cats', method: RequestMethod.GET }, //특정 경로를 제외할 수도 있다.
        { path: 'cats', method: RequestMethod.POST },
      )
  }
}
```

consumer.apply 메서드를 사용하여 Middleware를 등록하고 forRoutes 메서드를 사용하여 Middleware를 적용할 경로를 지정할 수 있으며,
Controller 클래스들도 사용할 수 있다. 또한 exclude 메서드를 통하여 특정경로를 제외할 수 있다.

### Functional Middleware

Functional Middleware는 함수 형태로 작성된 미들웨어 이다.

```typescript
export function logger(req: Request, res: Response, next: NextFunction) {
  console.log(`Request...`);
  next();
}
```

그리고 해당 Middleware를 사용하는 모듈에서 configure 메서드를 통해서 설정할 수 있다.

```typescript
consumer.apply(logger).forRoutes(CatsController);
```

### Multiple Middleware

apply 메서드를 통하여 여러개의 미들웨어를 사용할 수 있다.

```typescript
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```

### Global Middleware

등록된 모든경로에 미들웨어를 한번에 바인딩 하려면 use 인스턴스에서 제공하는 메서드를 사용하면 된다.
Global Middleware에서는 DI Container에 접근할 수없다. 이때 Functional Middleware를 사용할 수 있다.

```typescript
const app = await NestFactory.create(AppModule);
app.use(logger);
await app.listen(3000);
```
