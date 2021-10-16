# display
display 속성은 요소의 박스 레벨을 결정!  

- block : 블록 레벨처럼 렌더링
- inline : 인라인 레벨처럼 렌더링
    - marin과 padding값이 좌우로만 영향을 미침.
- inline-block : 인라인처럼(줄바꿈x 공백o) 배치되지만 블록의 성질을 가짐.
    - height, width 같은 속성을 가질 수 있음.

- none : visibility의 hidden과 차이점이 있음.
    - display none은 애초에 렌더링을 하지 않음.
    - visibility hidden은 렌더링하지만 화면에 보이지 않음(공간은 차지함)