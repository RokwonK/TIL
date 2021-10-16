# Tag의 종류

- b : 글자를 굵게
- i : 글자를 이탤릭체로
- u : 글자에 아래밑줄
- s : 글자에 중간선을 그어줌

<br>

- a  
앵커태그 속성
    - href : url을 씀
        - id를 이용하여 내부페이지에서의 이동이 가능함 => #(id이름)
    - target : 링크된 리소스를 어디에 표시할 것인지  
        - _self 현재 창에 로딩
        - _blank 탭을 만들어 로딩
    
<br>

- 요소를 묶기 위한 태그(자체의 의미는 없음)
    - div(블록 레벨 태그) : 줄을 다 가져감
    - span(인라인 레벨 태그) : 하나의 줄 안에서 부분만 따로 가져감

<br>

- 리스트 요소
    - 순서가 없는 리스트 : ul 점으로 표시됨
        - 각 리스트들을 li 태그로 표현
    - 순서가 있는 리스트 : ol 숫자로 표시됨
        - 각 리스트들을 li 태그로 표현
    - 용어와 정의 : dl
        - dt 용어
        - dd 설명(한 탭 들어가 있음)

<br>

- 이미지 태그 img
    - src 이미지 경로
    - alt 이미지의 대체 텍스트
    - width,height 크기를 고정

<br>

- 표 table  

    - caption 태그 : 표의 제목  
    - thead : 제목 행 표시  
    - tfoot : 바닥 행 표시  
    - tbody : 본문 행 표시  
    - colgroup : 모든 열의 값을 지정해 주고 싶을때  
        ㄴ col 각 열마다 나눠서 지정 해줄수 있음 span속성 => 범위
    - tr 하나의 행
    - td 행 안의 하나의 cell (제목 행은 th로 씀)
        - 속성  
        colspan="숫자" 우측 셀 병함  
        rowspan="숫자" 아래 셀 병합 하고 나서 병합된 셀 쪽은 작성x  

    웹접근성을 위한 기술  
    - headers 속성  
    th에 id를 쓰고 td의 headers에 해당 th의 id를 씀. (시각 장애인을 위함)

<br>

- Form 사용자의 입력

    - input의 속성  
        - type : text, password, radio, checkbox
            - radio,checkbox : 속성에 name이 똑같다면 같은 그룹, checked 속성 있다면 이미 선택 되어짐(radio도 마찬가지)
        - placeholder : 미리 적혀있는 것
        - file : 파일을 올릴때  
        - submit : 제출됨
        - reset : 값을 초기화
        - button : 버튼 빈태그가 아니므로 안에 원하는 글을 쓸 수 있음
        - image : src,alt 속성이 있음, 버튼임
        - select : 목록
            - option 안에 각각의 리스트를 넣어줌
            - selected 미리 결정해 놓은 것
        - textarea
            - cols 속성 넓이 몇칸
            - rows 속성 높이 몇줄
        - **label**  input의 이름을 붙여줌  
        속성 for에 내용을 적고 이어지는 input의 id에 같은 내용을 적음
        - fieldset : 여러 개의 폼 요소를 그룹화, 구조적을 나눔 ex) 필수, 추가 정보로 나눔
        - legend : filedset의 제목
        - form :   
        action => 폼 데이터를 처리하기 위한 서버의 주소  
        method => 데이터를 전송하는 방식(get, post)