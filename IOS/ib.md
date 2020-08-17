# IBOutlet, IBAction

**Interface Builder Outlet** => 배출구  
**Interface Builder Action** => 행동

- `IBOutlet`는 연결된 코드에서 값을 바꿀 시 인터페이스(화면)에 들어나는 것
- `IBAction`은 인터페이스에서 한 행동(버튼 터치 등)이 일어나면 연결된 코드에서 무언가 동작한다. 

<br>

## IBOutlet의 Storage

weak, strong이 있다.(그리고, reference count[참조 횟수] )  
**strong**은 그 IBOutlet을 소유하는 대상의 reference count를 증가(즉, 대상의 조상들의 뷰가 그 대상을 자체를 가진다). 이로써 좋은 점은 dealloc(소멸)이 되지 않고 가지고 있는다.(각 대상들이 count를 하나씩 가지고 있으므로 다 사라져야(그 뷰가 다 없어져야) dealloc된다) 문제는 그 만큼의 메모리르 잡아먹는다.    

**weak**는 reference count를 증가 시키는 것이 아니라 포인터를 소유함   

즉, 대상 뷰를 삭제하면 `weak`는 포인터이기 때문에 없어져 버리지만,  
`strong`은 조상들이 객체자체를 소유하고 있기 때문에 사라지지 않음.(전부 삭제하면 사라지겠지만)  

언제 필요할까? - 뷰를 삭제했다가 다시 넣어야 할때 strong을 사용한다고 한다. 말고는 weak를 사용하는 것이 좋을듯!

<br>

## IBOutlet에 값
값을 집어 넣고 싶다면 => 변수.text =  String을 넣으면 된다.

<br>

## IBAction의 행동
예를 들어 버튼의 클릭이 일어나면 연결된 함수가 실행됨.  
이 함수는 return 값이 없고 실행할 행동들을 정의해 주면됨.