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
















스키마 작성  
resolver?  

1. body-parser를 설치 - req를 파싱하기위한 모듈
2. apollo-server-express - graphql모듈 graphql-client를 사용하기 위함
3. graphql-tools - 스키마를 좀더 좋게 만드는 것

2에서 { graphExpress, graphiqlExpress}
두번째꺼는 ui상에서 확인해보기 위한 모듈임


3에서 { makeExecutableSchema }


```js

// Lang은 스키마
// Query는 쿼리임!

// 여기는 추상화된 데이터 타입임!
const typeDef = `
    type Lang {
        id: Int,
        name: String!
    }
    type Query {
        getLangs(name: String): [Lang]
    }
`
// 실제 데이터(예시)!
// 원래는 데이터베이스
const langs = [{
    id: 0,
    name: 'Node'
},{
    id: 1,
    name : 'Python'
}]


// 해결사
// 값을 보내는 용
const resolvers = {
    Query: {
        getLangs: () => langs
    }
}


// 실행할 수 있는 스키마를 만든다
const schema = makeExecutableSchema({
    typeDef,
    resolvers
})

server.use('/graphql', bodyParser.json(), graphExpress({
    schema
}))

server.use('/graphiql', graphiqlExpress({
    endpoint: '/graphiql'
}))

server.listen(port, () => console.log(port))
```

{
    getLangs{
        id,
        name
    }
}
// graphql의 GUI 툴 - graphiql 그 안에서 값 마음대로 넣어서 결과값 알 수 있음

```js

const resolvers = {
    Query: {
        getCountries(_, args
    }
}
```









Apollo란?

Apollo Client는 클라이언트에서
GraphQL과의 데이터 교환을 도움 - 리액트에서 사용하는 라이브러리는 React Apollo