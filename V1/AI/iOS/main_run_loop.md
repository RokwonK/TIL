# Main run loop

iOS의 `Main run loop`는 유저로부터 모든 input 이벤트를 받고, 적절한 응답을 해주는 것을 담당한다. 다음의 순서대로 Main run loop가 작동한다.  

1. User Event(tab 등)
2. OS가 인지
3. Event queue에 추가
4. Application
    1. Application Object  
    (이곳에서 input 이벤트를 해석, 그에 상응하는 Core object들 안에 있는 핸들러들을 호출함)
    2. Core object
    3. 핸들러들이 개발자들이 쓴 코드(메서드들)를 호출해준다.
5. 메서드가 반환되고 컨트롤은 main run loop로 돌아간다
6. Update Cycle의 시작!

여기서 `Update Cycle`는 이벤트 핸들링 코드를 수행하고 main run loop로 반환되는 시점이다.  
이 지점에서 View들을 배치(layout), 보여주고(display), 제약(constraints)한다.  
이 말은 메서드에서 view의 바뀐 값들을 이용하여 계산을 하지 못하고 그 전의 값들을 사용하여 계산을 할 수 있다는 것이다.( 메서드가 실행되고 뷰가 바뀌므로)
