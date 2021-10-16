# Media Query

Media(매체 screen, phone 등)에 따라 각 환경에 최적화된 화면을 제공하기 위함.  

- 구문  
@media 미디어 쿼리문{ ... (스티일 속성들)}  
    - @media : 미디어 쿼리의 시작
    - 미디어 쿼리문 => 이것이 참이면 뒤의 속성들을 적용
    - 미디어 쿼리문에는 미디어 타입과 미디어 특성이 옴
        - **미디어 타입**  
            미디어의 타입을 정의  
            screen, all, print, handheld 등이 있음.  
            보통 screen을 많이 사용
        - **미디어 특성**
            현 미디어의 크기 등 속성을 정의  
            width, orientaion 등이 있음.  
            width는 뷰포트의 너비, orientation은 가로인가(landscape), 세로인가(portrait)를 판단

- example
    - 하나의 미디어 쿼리에 여러 쿼리들을 함께 집어 넣을 수 있음.
    - only|not 미디어 타입 으로 선언 가능
    - (미디어특성 : 값)으로도 선언가능 위와 and로 합쳐서도 사용 가능  
    즉, 타입+특성 or 특성 or 타입 으로 사용할 수 있음, 물론 뒤에 계속 붙일 수도 있음(쭉 이어간다는 것).

```css

/*and 대신 ,을 쓰면 하나면 참이면 적용됨 */
@media screen and (min-width:200px) and(max-width:500px) {/*css 속성들*/}

```


- max-width와 min-width  
max-width로 선언된 값보다 이하일 경우 true  
min-width로 선언된 값보다 이상일 경우 true