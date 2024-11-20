---
title: Range, X-Total-Count
author: 김록원
date: 2022.09.27
category: software
---

# HTTP Header - Range, X-Total-Count

react-admin을 이용하여 어드민 페이지를 작성하는 도중 provider에서 List를 호출할때 `Range`와 `X-Total-Count`라는 HTTP 헤더를 사용하는 것을 확인하였다. 이름만 봐도 어느정도 추측은 가능하다

### Range
range는 요청헤더(즉, 클라이언트 측)에서 넣어 보내는 헤더로 `HTTP 메세지의 일부만 전송할 수 있도록 허용하는 기술`이다.  
대용량 파일 다운로드/중지/재시작, 페이지네이션에 유용하게 이용된다.  
'Range' : 'bytes=100-200'과 같이 리소스의 일부만을 서버에게 요청한다.  
서버 측에서는 응답헤더로 'Accept-Ranges' : 'Bytes'와 같은 응답을 보내주어야 브라우저에서 기능 지원을 확인할 수 있다.  

<br />

### X-Total-Count
응답 헤더에 들어가는 요소로 요청하는 리소스의 전체 갯수를 넣어보냅니다.  
'X-Total-Count' : '123'

<br />

### 마무리
REST API를 사용하지만 제대로 Restful하게 사용하지는 못했던 것 같다. 제대로 배웠으니 잘 활용해보도록 하자.