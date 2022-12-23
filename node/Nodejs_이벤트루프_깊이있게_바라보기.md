
---
title: npm, yarn, pnpm
author: 김록원
date: 2022.12.20
category: node
---

# Nodejs 이벤트루프 깊이있게 바라보기  
javascript Deep Dive 책을 공부하면서 이벤트 루프에 관련해 부분적으로 알게 되었다.  
전체적인 동작방식을 이미지와 함께 쉽게 알려줘서 큰 흐름을 이해하기에 좋았고 만족스러웠다.  
이벤트 루프의 보다 정확한 동작방식에 대해 더 알고 싶은 마음에 공식문서와 잘 정리해준 여러 블로그들을 참조하여 이해한 내용을 정리해보았다.  
(틀린 부분이 있다면 피드백 해주시면 감사하겠습니다)  

<br />  

## Node.js를 이루는 구조
이벤트 루프를 이해하기 전에 Node.js가 어떻게 이루어져 있는지, 각 기능이 어떤역할을 하는지 살펴보자.  

각 구조를 간단하게 알아보자면
- Node.js Core Library(console, 파일핸들링 같은 라이브러리)
- Node.js Bindings(socket, http 관련 라이브러리)
- V8(js 실행 엔진)
- libUV(비동기 IO 라이브러리)

<br /><hr><br />

## Node.js 이벤트 루프와 태스크 큐
- 이벤트 루프
  - 추상적인 개념
  - 비동기 로직을 여러 페이즈로 나누어서 처리하는 동작방식 그 자체라고 얘기할 수 있음
  - 여러 페이즈는 어떠한 비동기 로직이냐에 따라 우선순위가 정해져 있음
  - 이 동작방식은 js엔진과 함께 싱글쓰레드로 동작하며 콜 스택이 비어있을때 동작하게 됨
- 태스크 큐
  - 태스크 큐는 여러 페이즈(여러 큐라고 말은 못하는게 각 페이즈를 처리하는 것들이 전부 큐인지는 모름)로 나누어져 있음
  - 테스크 큐의 각페이즈는 어떤 역할을 하는지
  - 마이크로태스크큐, 넥스트틱큐, 매크로태스크큐  

<br />  

### Node.js 시스템 이미지
- [이미지 출처](http://stackoverflow.com/questions/10680601/nodejs-event-loop)

<br /><hr><br />

## microTaskQueue와 nextTickQueue
microTaskQueue와 nestTickQueue는 위에서 설명한 태스크큐(macroTaskQueue 라고 부름)에 포함되지 않는다.  




<br /><hr><br />

## 더 정리해보기
### Node.js는 싱글쓰레드인가?
Node.js는 js코드는 싱글쓰레드로 처리하되 비동기 IO에 한해 멀티 쓰레드를 이용한다.  
즉, 프로그래머가 조작하고 js코드가 실행하는 과정자체는 오직 싱글 쓰레드로 동작한다.(내부 동작 - libUV가 진행하는 동작은 멀티쓰레드가 이용될 수 있다.)  

js엔진에서 쓰레드 한개 이벤트 루프가 쓰레드한개를 잡아서 돌아간다고 생각할 수 있다.(필자가 그랬다.)  
하지만 실제로 js엔진과 이벤트루프는 하나의 쓰레드로 돌아간다.

<br />  

### 




<br /><hr><br />

## 참고자료
- [Node.js 공식문서](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)
- [로우 레벨로 살펴보는 Node.js 이벤트 루프](https://evan-moon.github.io/2019/08/01/nodejs-event-loop-workflow/)
- [NodeJS Event Loop파헤치기](https://medium.com/zigbang/nodejs-event-loop%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-16e9290f2b30)
- [Node.js 이벤트 루프(Event Loop) 샅샅이 분석하기](https://www.korecmblog.com/node-js-event-loop/)
- [이벤트 루프와 태스크 큐 (마이크로 태스크, 매크로 태스크)](https://velog.io/@yejineee/%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EC%99%80-%ED%83%9C%EC%8A%A4%ED%81%AC-%ED%81%90-%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C-%ED%83%9C%EC%8A%A4%ED%81%AC-%EB%A7%A4%ED%81%AC%EB%A1%9C-%ED%83%9C%EC%8A%A4%ED%81%AC-g6f0joxx)
- [Node.js를 이루는 코어 라이브러리들](https://shin-bugkiller.tistory.com/2)
- [Row level Node : js의 동작 방식부터 libuv와 event loop까지](https://darrengwon.tistory.com/953)
- [이벤트 루프와 태스크 큐 (마이크로 태스크, 매크로 태스크)](https://whales.tistory.com/130)

