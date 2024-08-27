---
layout: post
title: "[NestJS]- NestJs Pipes"
categories: NestJS
tags: [NestJS, NodeJS, Typescript, Pipes]
---

## Pipes

<span style = "color:#FF8C00">Pipes</span>는 @Injectable() 데코레이터로 주석이 달린 클래스이다.

Pipes는 두가지 사용 방법이 있다.

- Transformation(변환) : 입력데이터를 원하는 형식으로 변환 할 수 있다.
- Validation(유효성 검사) : 입력 데이터의 유효성을 확인하고 유효한 경우 데이터를 전달하고, 유효하지 않다면 예외를 넘긴다(Throw).

URL 요청이 왔을 때, 해당 URL에 대한 데이터들에 대한 처리를 하며 옳지 않은 데이터가 온다면 Error 처리를 하고 그렇지 않다면 데이터를 처리한 후 handler에게 간다.

위의 두가지 사용 방법 모두 Controller의 Handler에 들어오는 인수를 기반으로 수행된다. Nest 메서드가 호출되기 전에 파이프를 통하여 메서드로 이동되는 데이터를 확인하는 방식이다.
즉, Nest의 handler가 호출 되기 이전에 Pipe에서 들어오는 데이터(인수)를 받아 정의된 Pipe가 작동되어 검증을 하는 작업이다.

Pipe는 에외계층(exceptions zone)에서 작동 되기 때문에 만약 pipe가 throw를 하면 controller에서의 메서드는 실행되지 않는다.

![NestJs](/assets/images/Pipe_1.png)
<small>출처:<https://docs.nestjs.com/pipes></small>

## Binding Pipes

파이프 클래스의 인스턴스를 적절한 Context에 바인딩하여 파이프를 사용할 수 있다.

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">Handler-level Pipes</span><br/>
Hanlder-level 에서 @UsePipes() 데코레이터를 사용하여 Pipe를 적용할 수 있으며, 이 Handler의 모든 파라미터에 Pipe가 적용이 된다.

```typescript
@Post()
@UsePipes(manlyPipe)
createManly(@Body() data: manlyDTO){
    //...
}
```

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">Parameter-level Pipes</span><br/>
Hanlder 전체가 아니라 하나의 Parameter만 Pipe를 적용 시키는 것으로 @Param(), @Qurey(), @Body()와 같은 데코레이터에 사용하여 Pipe를 적용시킬 수 있다.

```typescript
@Get(':dream')
async findOne(@Param('dream', manlyPipe) dream: string) {
  //...
}
```

<span style = "font-weight:bold;font-size:20px;color:#CC8C00">Global-level Pipes</span><br/>
Global-level Pipes는 애플리케이션 레벨의 Pipe로 모든 route handler에 적용이 된다. 애플리케이션의 bootstrap에 useGlobalPipes() 메서드를 사용하여 Pipe를 적용할 수 있다.

```typescript
const app = await NestFactory.create(AppModule);
app.useGlobalPipes(new MyPipe());
await app.listen(3000);
```

## Pipes Exception Handling

Nest에서 Pipe를 이용하여 예외를 처리할 수 있는데, Pipe에서 예외를 발생 시키면 Nest 자체에서 자동적으로 예외 필터를 호출하여 예외를 처리 한다.

```typescript
import { Injectable, PipeTransform, ArgumentMetadata, BadRequestException } from '@nestjs/common';

@Injectable()
export class ManlyPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    if (!value) {
      throw new BadRequestException('Not Find Manly...');
    }
    return value;
  }
}

@Get(':manly')
async find(@Param('manly', ManlyPipe) manly: string) {
  return this.manlyService.findOne(manly);
}
```

ManlyPipe는 PipeTransform 인터페이스를 구현하여 작성한다.

```typescript
//PipeTransfrom
export interface PipeTransform<T = any, R = any> {
  transform(value: T, metadata: ArgumentMetadata): R;
}
```

PipeTransform 인터페이스에 transform() 메서드를 이용하는데, value가 없으면 BadRequestException을 호출하여 예외를 발생 시킨다.
Param인 manly 파라미터가 null이거나 undefined 즉, 없다면 Nest 자체가 예외필터를 호출 하여 예외를 처리 하게 된다.
