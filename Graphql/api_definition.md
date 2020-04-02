# 엔드포이트가 하나면 API 정의를 어떻게?

- resolver functions개념을 사용하여 클라이언트가 요청한 데이터를 수집  

- API에 노출 된 모든 유형(즉, 클라이언트에서 사용할 API들)은  
<span style="color:red">GraphQL 스키마 정의 언어(SDL)</span> 를 사용하여 스키마에 기록됨!  
    ㄴ 클라이와 서버간의 계약 => **클라이언트가 데이터에 액세스하는 방법을 정의**

## SDL의 장점
- 스키마가 정의 되면 프론트와 백엔드가 네트워크로 전송되는 데이터의 명확한 구조를 알고있으므로 추가 작없 수행 상관x  
(즉, 스키마가 정의되면 프론트와 백엔드가 독립적으로 일 가능!)
- 협업 시 쓸데없는 시간소요 줄임

## SDL(Schema Definition Language) 
- API의 스키마(즉, 틀)을 정의함

```javascript
type Person{
    name: String!
    age: Int!
}
```
