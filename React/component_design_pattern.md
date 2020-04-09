# Component_design_pattern


1. 기본 컴포넌트 => 일일히 작성해야하는것을 컴포넌트로 규격을 맞춰줌

2. 고차 컴포넌트 HOC(Higher Order Component) => input으로 component를 받아 그 컴포넌트를 감싸는 component를 만드는 것

4. Presentational & Container 컴포넌트 =>  
컴포넌트가 화면에 대한 정의를 넘어서 **데이터 fetching**(네트워크 요청, localstorage사용)까지 한다면 복잡해짐 그래서 나누는 것! 

    한 컴포넌트 내에 존재하는  
    화면 정의하는 로직 : Presentational컴포넌트  
    데이터와 관련된 로직 : Container컴포넌트로 분리하는 것  


    즉,  
    Presentation에는  
    - JSX가 있고,
    - render에 필요한 **데이터**이미 존재(Container 컴포넌트)하는 이미 있다고 가정
    - UI를 위한 state는 존재!

    Container에는  
    - JSX가 거의 없음
    - 네트워크 요청(Ajax), HOC등을 이용해 render에 필요한 데이터를 Fetching함
    - 데이터 Fetching을 위한 state가 존재

