Nestjs는 효율적이고 확장가능한 Node.js 서버 어플리케이션이다.  
객체지향, 함수형, 함수반응형 프로그래밍 방법을 지원한다.

내부적으로 `Express`를 사용하고 있으며 `Fastify`도 간간히 사용한다.
두 프레임워크보다 높은 수준의 추상화를 제공하지만 API는 직접적으로 노출하기도 한다.

프레임워크 자체에 여러 라이브러리들을 가지고 있어 자유롭게 사용할 수 있으며 개발자가 라이브러리 선택에 있어 들이는 시간을 단축시킬 수 있다.

즉시사용 가능한 어플리케이션을 제공한다(테스트하기 쉽고, 확장 가능하며, 느슨하게 결합되어 있으며, 유지보수하기 쉽다)

nest cli를 이용하여 기초 Frame을 만들 수 있다.

```bash
npm i -g @nestjs/cli
nest new {project-name}
```

### Philosophy

- node.js가 규칙, 패턴 그리고 방법이 있게(Node.js의 단점은 하고 싶은대로 다 할 수 잇음 - 아무런 룰도 없다는 뜻, 통일성이 없어서 구조를 잡기가 힘듬)
- 기본적으로 json 형식으로 값을 받고 json 형식으로 리턴 가능하다.(express는 지정해줘야함)
- path가 `:id` 보다 코드에서 밑에 있으면 search를 id로 판단한다.
- Pipe(Express의 미들웨어 같은 것) 중 유효성검사를 해주는 ValidationPipe 사용가능
- 허용되지 않은 값 막기, query, param 등의 형변환이 가능하다.(Nest 프레임워크의 유용성)

### Start

- nest.js는 기본적으로 main.ts를 불러온다.
- in `src/main.ts`
    
    ```tsx
    // NestFactory 객체 생성 INestApplication 인터페이스를 수행하는 어플리케이션 객체를 반환함
    NestFactory.create(AppModule) 
    ```
  

<br />
<br />

# 주요 기능

## Controllers

요청을 처리하고 응답을 반환하는 역할을 하는 곳(`@Controller` 데코레이터 사용해서 클래스가 Controller 역할을 한다는 것을 알린다.

`@Get(’/path’)` , `@Post(’/path’)` 와 같이 HTTP Method를 데코레이터 형태로 메서드 위에 붙이면 그 메서드는 해당 Method의 handler로 동작한다.(데코레이터를 사용해서 각 함수마다 라우팅 역할을 지정해준다)

```tsx
@Controller()
export class AppController {

	@Get(”/hello”)
	sayHello(): string {
		return 'Hello';
	}
}
```

`@Req` 데코레이터를 이용해 요청객체를 직접 받을 수 있다.
그러나 Nest에서 직접적으로 요청객체를 다룰 일은 거의 없고 요청에 포함된 쿼리, 패스, 본문을 쉽게 받을 수 있도록 `@Query()` , `@Param(key?: string)` , `@Body` 데코레이터를 제공한다.

```tsx
 import { Request } from 'express';
import { Controller, Get, Req } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(@Req() req: Request): string {
    console.log(req);
    return this.appService.getHello();
  }
}
```

`@HttpCode(…)` 데코레이터를 이용해 메서드에 응답 상태코드를 지정할 수 있다.

```tsx
@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```

`@Param('이름')` 을 이용해서 패스(라우트) 파라미터를 전달 받을 수 있다. 객체로 한 번 받을 수 있는 방법과 따로따로 받을 수 있는 방법 2가지를 지원한다.

```tsx
// 객체 형식
@Delete(':userId/memo/:memoId')
deleteUserMemo(@Param() params: { [key: string]: string }) {
  return `userId: ${params.userId}, memoId: ${params.memoId}`;
}

// 각 변수로
@Delete(':userId/memo/:memoId')
deleteUserMemo(
  @Param('userId') userId: string,
  @Param('memoId') memoId: string,
) {
  return `userId: ${userId}, memoId: ${memoId}`;
}
```

`@Body()` 를 이용하여 POST요청에 대한 payloads를 받을 수 있다. 이때 DTO 객체를 선언하여 키와 값 타입을 정의해야 한다.  

```tsx
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}

@Post()
async create(@Body() createCatDto: CreateCatDto) {
  return 'This action adds a new cat';
}
```

이 외에도 응답헤더 커스텀을 위해 `@Header(’헤더이름’, ‘값’)` 을 사용할 수 있다.
하위도메인에 대한 라우팅도 제공해준다. `@Controller({host ‘api.example.com’})` 

<br />
<hr>
<br />

## Providers

`Providers`는 Nest의 근본이 되는 개념이다.
Nest의 대부분의 클래스들은 provider로 다뤄진다.(privider의 종류 - service, repository, facotry, helper 등)

`Providers`에서 가장 중요한 개념은 **의존성을 주입**할 수 있다는 것이다.
이는 객체가 다른 객체들과 다양한 관계를 만드는 것과 객체의 생명주기의 관리를 Nest의 런타임 시스템에 맡긴다는 뜻과 같다.
(`Moudule` 내 providers에 주입할 class들을 선언하면 끝! - Nest의 런타임이 위에 설명한 관리를 해줌)

### Service

주입 객체로 가장 많이 사용하는 `Service`를 보자. 앞서 설명한 `Controller` 가 요청 및 응답을 처리해주는 역할을 한다면 기본적으로 `Service` 는 I/O 처리 로직을 담당한다.(데이터베이스 접근, 네트워크 API 처리 등)

```tsx
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```

class 선언 위에  `@Injectable()` 데코레이터가 붙은 것을 볼 수 있는데 이는 **Nest의 IoC Container**에 의해 관리되어 질 수 있는 class라는 것을 명시하는 것이다.
`Controller` 에서는 constructor를 통해 주입된 `Service`를 사용할 수 있다.
(추가적으로 해당 Service를 명시적으로 등록해주어야 하는데 아래의 ‘Provider 등록하기’ 파트에 설명되어 있다.)

```tsx
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

### Provider로 등록하기

`Controller`에 `Service`를 주입해서 사용하기 위해서는 시스템에 등록해주어야 한다.
두 클래스 모두 `@Module` 데코레이터 안에 등록할 수 있다.

controllers에 있는 `Controller` 들에게 providers에 작성된 `Provider` 들을 주입시킨다는 의미이다.

```tsx
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```

이렇게 작성된 코드는 Nest의 **IoC Container가 관리**하여 클래스의 생명주기 및 의존성 주입(Service를 Controller에게)을 관리해준다!(Nest 초강점)

### Nest의 Dependency Injections

Nest에서는 TypeScript의 기능(타입 작성 기능) 덕분에 종속성을 관리하기가 매우 쉽다.
웨에서 보았듯이 주입 받는 대상의 constructor에 타입을 작성해주기만 하면 된다.

### Provider의 더 알아보기

provider들의 생명주기는 기본적으로 앱의 생명주기와 동일하다.
Nest 앱이 bootstrapped 될때, 모든 의존성이 생기고 모든 provider들이 초기화된다.
당영하게도 앱이 종료될때 각 provider도 전부 종료된다.
하지만, **provider의 수명주기를 수정할 수 있는 방법**도 제공해준다.([https://docs.nestjs.com/fundamentals/injection-scopes](https://docs.nestjs.com/fundamentals/injection-scopes))

뿐만 아니라 **provider를 custom 할 수 있는 방법**도 제공해준다.
([https://docs.nestjs.com/fundamentals/custom-providers](https://docs.nestjs.com/fundamentals/custom-providers))

**Optional 가능**

반드시 해결할 필요가 없는 의존성이 존재할 수도 있다.
예를 들어, class가 config객체에 따라 설정될 수도 있지만 그렇지 않은 경우도 있을 것이다. Optional 기능이없다면 config객체의 기본값이 반드시 추가되어야 한다. 이렇듯 반드시 해결할 필요가 없는 의존성에 대해서 Optional 기능을 추가할 수 있다.

```tsx
import { Injectable, Optional, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  constructor(@Optional() @Inject('HTTP_OPTIONS') private httpClient: T) {}
}
```

**Property-based injection**

지금까지는 constructor 메서드에서만 주입을 받았다. 하지만 어떠한 경우에는 property에 의존성을 주입하는게 효율적이다. 예를 들어 어떤 최상위 클래스가 하나 또는 여러개의 프로바이더에 의존하는 경우 하위 클래스에서 super()를 호출하여 끝까지 전달하는 것은 매우 귀찮은 일일 것이다. 이때 property에 `@Inject()` 데코레이터를 적용시키며 해당 상황을 해결할 수 있다. property에 주입시킴으로써 super를 호출할 필요없고 보다 직관적인 코드를 구현할 수 있다.

```tsx
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  @Inject('HTTP_OPTIONS')
  private readonly httpClient: T;
}
```