# Margin의 숨겨진 지식/padding과의 차이점


- **margin : 0 0 0 0**  
상 우 하 좌 순으로 값이 매겨짐.  
3개만 쓴다면 좌는 우의 값을 따르고,  
2개만 쓴다면 하는 상의 값을 따른다.  
하나만 쓴다면? 모든 방향이 동일한 값을 가진다.  
padding 또한 같음.


- **margin : auto**  
브라우저에 의해 계산된 값이 적용됨.(width값이 있고, 좌우가 auto라면 수평정렬이 됨)  
위와 같이 네방향 모두 선언가능  
padding에는 없고 margin에만 있는 기능  
ex) margin : 0 auto => 수평정렬됨(물론 width값이 존재할 경우)


- **margin callapse**
두 블록의 상하 margin이 겹칠 경우 더 큰 margin만 적용됨(정확히는 먹어버림 냠).  


- **padding과의 또다른 차이점**  
    1. +는 당연가능하고, -값을 넣을 수 있음 => 줄어드는 것.  
    2. margin은 테두리 바깥쪽 부분, padding은 안쪽 부분임


- **%**  
%의 기준은 width이다. 상,하도 width의 기준으로 %를 가짐  
자식의 %는 부모의 콘텐츠영역의 비례값

- **width**값은 border를 포함한 값이다.