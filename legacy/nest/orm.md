---
title: ORM이란?
author: 김록원
date: 2022.09.28
category: node
---

# ORM이란? TypeORM, Knex, Prisma

데이터베이스에서 데이터를 읽어올때 보통 많은 개발자들이 ORM을 많이 사용한다.
ORM이란 무엇일까?

ORM이란 Object Relational Mapping의 줄임말로 객체형태의 코드와 데이터베이스의 데이터를 일치시켜주는 도구이다.
- **RDB의 모델을 OOP의 객체 형태로 투영시키는 방식**
- **객체 모델과 관계형 모델간에 불일치가 존재하는데 객체간의 관계를 바탕으로 SQL을 자동생성하여 불일치를 해결해 주는 것**이 ORM의 역할이다

ORM = Object ← 매핑 → DB

### ORM의 장점

- 객체 지향적 코드
    - Model별로 코드를 작성하여 직관적, 가독성이 높음
    - 긴 SQL 문장을 작성할 필요없음
- 재사용 및 유지보수의 편리성
    - ERD를 보는 것에 대한 의존도가 낮아짐
    - 해당 객체들은 재사용이 가능함
- DBMS에 대한 종속성
    - ORM이 DB에 맞는 SQL로 바꾸어주기 때문에

### ORM의 단점

- 완벽한 ORM만으로 구현하기 힘듬
    - 설계에 매우 신중해야함(왜 그래야할까?)
    - 복잡해질수록 심각한 속도 저하(SQL로 변환하는 과정에서 쓸데 없이 길어지는 SQL)
- SQL이 ORM의 배우게 됨(라이브러리 의존성 증가)
    - 진짜 SQL 형식을 까먹을 수 있음

<hr>

## Node.js ORM의 종류
N+1 문제 - 연관 관계가 설정된 엔티티를 조회할 경우에 조회된 데이터 갯수(n) 만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오게 된다.

### Sequalize
- 쿼리를 객체 형식으로 표현하기 때문에 길어질 시 직관적이지 못하고 가독성이 떨어짐.
- TypeORM과 비교했을때 속도가 현저히 떨어짐

### TypeORM
- 체이닝 방식으로 쿼리 작성 가능(가독성 업)
- Active Record Pattern, Data Mapper Pattern을 지원함(모델 내에서 메서드 정의, 레포지토리 라는 별도의 클래스에서 메서드 정의)

### Knex
raw SQL 쿼리를 작성하는 것보다는 편리하지만 다른 ORM을 사용하는 것보다는 불편함.
문자열을 연결해서 SQL 쿼리를 만드는 것보다 더 편리한 방식으로 동적 쿼리를 코드로 생성가능함 (쿼리 빌더 수준).
- 프로그래밍 언어의 클래스, 함수, 변수 등을 사용하면서 쿼리를 작성, raw query의 단점을 보완할 수 있다.
- 단점을 보완하되 쿼리 빌딩은 복잡하지 않기 때문에 쿼리 수행 시간이 크게 다르지 않다.
- 쿼리 재사용성
- 쌩 쿼리가 굉장히 비슷함

### Prisma
기존 ORM들과 같은 문제점을 해결하기 위함.
- 비대해진 모델 해결
- 예측할 수 없는 쿼리 발생(lazy loading에서)
- TypeORM에 비교하여  generic Operator(연산자) 를 제공 (filtering 등)    
- 어플리케이션이 커지면 유지, 관리에 TypeORM보다 안전성있음

```jsx
model User {
  id    Int     @id @default(autoincrement())
  name  String?
  email String  @unique
  posts Post[]
}

model Post {
  id        Int     @id @default(autoincrement())
  title     String
  content   String?
  published Boolean @default(false)
  authorId  Int?
  author    User?   @relation(fields: [authorId], references: [id])
}
```

```jsx
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  OneToMany,
  ManyToOne,
} from 'typeorm'

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number

  @Column({ nullable: true })
  name: string

  @Column({ unique: true })
  email: string

  @OneToMany((type) => Post, (post) => post.author)
  posts: Post[]
}

@Entity()
export class Post {
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  title: string

  @Column({ nullable: true })
  content: string

  @Column({ default: false })
  published: boolean

  @ManyToOne((type) => User, (user) => user.posts)
  author: User
}
```