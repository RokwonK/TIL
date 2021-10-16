# Thread

쓰레드에는 두가지가 있다.  
1. 소프트웨어적으로 나누는 Thread
2. 하드웨어적으로 나누는 Hyper-Thread   

## Thread
- 하나의 프로세스 안에 존재, 실행하는 단위 (하나 혹은 여러개일 수도 있음.)  
- 일부 메모리를 서로 공유함.(같은 프로세스 안에 있으므로)
- 힙 공간(전역 변수), 파일들, Data영역(프로그램 변수, static 변수), 입출력 상태를 공유
- 고유의 stack, Code영역(작성한 코드들이 할당되는 메모리) 및 cpu state들을 가짐

stack 및 cpu state들만을 save 및 load 하기 때문에 상대적으로 프로세스 간의 Context Swithing보다 빠름  




<br>

## Hyper-Thread
- 하드웨어로 Context Switching을 도와줌
- 하나의 cpu안에 논리적으로 쓰레드를 나눔
    - 이뜻은 하나의 cpu안에서 그냥 Thread가 hyper-Thread의 수만큼 돌아갈 수 있다는 뜻임
    - 즉, register set을 Hyper-Thread 수 만큼 가짐.
    - 그러므로 쓰레드의 Context Swithing 시, cpu state의 save, load가 필요없음, 그냥 넘어가면됨
- cpu 정보를 저장 및 가져오는 시간을 줄이기 위한 하나의 방법