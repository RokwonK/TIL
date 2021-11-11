# Functor와 Monad  

함수형 프로그래밍에 등장하는 두 용어의 의미는 무엇일까?  

### **순서**  
1. Functor와 Monad를 이해하기 앞서 - Context와 Content  
2. Mapping
3. Functor
4. Monad

<br>

### **Functor와 Monad를 이해하기 앞서 - Context와 Content**  
- **컨텍스트**는 그릇이라고 할 수 있고,  
**컨텐트**는 그 그릇에 담긴 것이라고 할 수 있다.  
- Swift의 Optional을 컨텍스트,  
Optional로 래핑된 타입이 컨텐트라고 볼 수 있는 것이다.  

**한 발 더 나아가 Container에 대해서도 알아보자**  
- 한 개 이상의 원소(혹은 경우)를 가질 수 있는 것 - **Container**  
- Array, Set, Dictionary를 예시로 들 수 있다.  
- Optional은 값이 있을 경우, 없을 경우도 있으므로 이 또한 Container라고 부를 수 있을 것이다. 

<br>  

### **Mapping**  
원소 값에 특정한 변형을 적용하는 것 - 변형을 사상한다  
컨테이너에 매핑연산을 수행할 수 있음.  

이렇게 Mapping 할 수 있는 모든 것들을 *Functor*라고 부른다.  
그렇다면 `Container == Functor` 일까?  

<br>  

### **Functor**  
Functor에서 매핑 연산이 일어나는 과정
1. 컨텍스트(컨테이너)에서 값을 빼냄
2. 넘겨준 함수로 값을 변경
3. 컨텍스트에 다시 넣어줌
4. 변경된 값을 반환함
