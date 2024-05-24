---
layout: post
title: " [NestJS]- NestJs Exception filters"
categories: NestJS
tags: [NestJS, NodeJS, Typescript, Exception filters]
---

## Middleware

Nest에는 애플리케이션 전체에서 처리되지 않은 모든 예외를 처리할때 <span style = "color:#FF8C00">Exception filters</span>를 사용해볼 수 있다.

예외처리는 프로그래머가 예상치 못한 예외의 발생에 미리 대처하는 코드를 작성하는 것으로, 이를통해 실행 중인 프로그램의 비정상적인 종료를 막고, 프로그램의 상태를 정상적으로 유지하기 위해 사용한다.

Node.js에서는 throw new Error()와 같은 방식을 통해서 예외 처리를 진행하지만 Nest에서는 HttpException() 이라는 인터페이스를 사용하여 예외를 처리한다.

![NestJs](/assets/images/Filter_1.png)
<small>출처:<https://docs.nestjs.com/exception-filters></small>

Nest에서는 이러한 모든 처리되지않은 예외들 unhandled exceptions들을 exceptions layer에서 처리하고 자동으로 아래와 같은 response를 응답해준다

```typescript
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

### Standard Exceptions

```typescript
@Get()
async findAll() {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

위와같이 Nest에서는 예외처리를할때 HttpException 인터페이스를 사용하여 예외처리를 해주면 된다.
그러면 아래와 같이 response가 온다.

```typescript
{
  "statusCode": 403,
  "message": "Forbidden"
}
```

- statusCode: 인수에 제공된 HTTP 상태 코드 기본값
- message: HTTP 오류에 대한 간략한 설명

그리고 response에 message 부분을 변경하려면 인수에 다른 문자열을 넣어도 좋다. 다른 Class 파일에 미리 오류 message들을 정의해놓고
사용하는 방법도 좋은 방법이다.

```typescript
@Get()
async findAll() {
  throw new HttpException(CODE_CONSTANT.EXIST_DATA, HttpStatus.FORBIDDEN);
}
```

## Exception Filters

기본 exceptions filter가 대부분의 많은 예외를 자동적으로 처리해주지만 layer 자체를 컨트롤 할 수도 있다.
클라이언트에게 보내는 응답의 내용과 flow를 제어할 수 있다.

```typescript
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
    });
  }
}
```

@Catch 데코레이터에서 filter가 HttpException에 해당되는 예외가 있는지 찾으며, 파라미터가 여러개가 될수 있다.
ctx는 실행환경을 의미하고, exception.getStatus() 메서드를 통해 예외에 해당하는 http 상태코드를 받을 수있다.

### Binding Filters

이후 작성한 exception filter를 적용할때는 @UseFilters() 데코레이터를 사용하면 된다.

```typescript
@Post()
@UseFilters(HttpExceptionFilter)
async create(@Body() createDTO: CreateDTO){
    throw new ForbiddenException()
}
```

@UseFilter() 데코레이터는 controller 전체에도 적용할 수 있다.

```typescript
@Controller("member")
@UseFilters(HttpExceptionFilter)
export class MemberController {}
```

또한 애플리케이션 전역에 적용되는 global-scoped filter를 생성하기 위해서는 아래와 같이 적용 할 수 있다.

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new HttpExceptionFilter());
  await app.listen(3000);
}
bootstrap();
```
