# setNeedsLayout과 layoutIfNeeded

iOS App의 동작흐름을 먼저 보자.  
1. 유저 인터렉션(터치 등) 이벤트 발생
2. Cocoa Framework의 UI객체 들을 거쳐 OS로 전달
3. OS는 port를 통해 Event queue로 보냄.
4. main run loop가 돌면서 Event queue에서 하나씩 빼내 처리함
5. main run loop가 이벤트를 빼네 App 

<br>

main run loop는 언제부터 동작하기 시작하냐?  
> App 실행시 UIApplication이 메인 스레드에서 main run loop를 실행