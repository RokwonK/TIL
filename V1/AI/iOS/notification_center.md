# Notification Center

Notification Center - 직역하면 공고 공간(말 그대로 공지해주는 공간이다)  
이 Center안에 등록된 이벤트가 발생 시, 그에 연결된 행동을 취한다.  
완전히 분리된 두 뷰에서 한 뷰에서의 일어난 행동에 대해 다른 뷰에서도 그에 대한 행동을 취할 수 있게, 연결해주는 `중계자 역할`을 한다.  

- notification center에 값들을 저장.
- 이 값의 변경에 따라 뷰들이 바뀌는 곳에 Observer(notification이 이 옵저버에게 행동을 전달한다!)를 심어놓는다.

1. 이벤트가 일어날 수 있는 곳에서 post(값을 바꾸기)가 일어난다.
2. Notification Center는 이 이벤트가 일어나면 Observer가 심어진 곳에 알림
3. 각 뷰들은 observer로 이벤트가 발생했다는 것을 듣고 그에 따른 행동을 취함!

<br>
<br>

*참고 URL - [https://jinshine.github.io/2018/07/05/iOS/NotificationCenter/](https://jinshine.github.io/2018/07/05/iOS/NotificationCenter/)*