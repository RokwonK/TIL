# Redis

redis.io

redis란? RAM(Random Excess Memory) 상에서
원하는 데이터 타입을 정의하여서 하드디스크 상에서 가져오는 것이 아닌
매우 빠르게 in memory cache(ram)상에서 redis가 운용되기 때문에

무조건 적으로 좋은가?
모든 경우에서 사용할 순 없음!
RAM은 휘발성 메모리!(컴 종료시 RAM이 다 날라감)
즉, 초기화되어도, 삭제되어도 문제가 없는 데이터에 대해서만 redis로 활용해야함

사용사례 - 회원 관리 - 회원 로그인이후 세션을 관리할때

세션은 삭제가 되어도 문제가 없음 - 특별한 데이터 x 새로 생성해도 큰 문제가 없음
로그인 되면 생성 로그아웃되면 삭제됨
데이터가 초기화 되어도 되는 데이터에 대해서
중요한 정보를 가지고 있는 것에는 redis 사용x


docker상에서도 사용가능함!

홈페이지에 설치 나와 있음

wget ~~~(다운)
tar xzf ~~~(압축 해제)
cd red~~ (알아서 설치됨) - 학습을 위해서 노트북상(로컬)에서 빌드
도커를 사용한다면 - 다른 방법

1. redis-server 입력하면 실행됨
2. npm init
3. sudo npm i redis --save

```js
    const redis = require("redis");
    const client = redis.createClient();

    // 연결, 연결되었으면 콘솔로 확인
    client.on('connect', () => {
        console.log('connected')
    })

    // key,value 로 값을 지정
    client.set('backend','node', (err, res) => {
        console.log(res)
    })

    //20초 뒤에 만료
    client.expire('backend', 20)

    //값을 받아옴
    client.get('backend', (err, res) => {
        console.log(res)
    })

    // ram에 저금하는 것 삭제 => 비동기
    client.del('backend', (err, res => {
        if(err) {
            console.log(err)
        }
        console.log(res)
    }))

    
    client.set('frontend', 1, () => {
        //increment 값을 증가
        // ram에 접근하는 것이므로 비동기
        client.incr('frontend',(err,set) => {
            if (err) {
                console.error(err)
            }
            console.log(res)
        })
    })

    client.set('full', 2, () => {
        //decrement
        // ram에 접근하는 것이므로 비동기
        client.decr('full',(err, res) => {
            
        } )
    })
```




publisher - subscrib??? 
데이터가 변경되거나 update가 되는 내용을 client에 전달하는 패턴
- 출간자가 구독자에게!

redis server 실행해 놔야됨
```js
const express; 
const redis;

const publisher = redis.createClient()
const app = express()

app.get('/', (req,res) => {
    const data = {
        full: 'node'
    }

    //subscirber에거 바꼈다고 알려줌
    // 보내면 자동으로 subscriber는 자동으로 받게됨(구독햇으니 알림설정을 받는 것)
    publisher.publish("subscirber.notify", JSON.stringfy() )
    res.send("Publisher sent an event via Redis")
})

app.listen(port, () => "Running at PORT 8000")
```

eventemitter???

subscribe
```js

const express;
const redis;
const subscriber = redis.createClient();

subscriber.on('message', (channel, message) => {
    console.log(message)
})

subscirber.subscribe('subscriber-notify')

const app = express()
let count = 0

app.get('/', (req,res) => {
    res.send(`sub ${++count}`)
})

app.listen(6000, () => {
    console.log('Running at port')
})
```
신문사는 하나 구독자는 여럿!


redis cache를 확인함(얼마나 빠른지 postman으로 확인해보기)


res.json({type:'dfd', data:JSON.parse(res) })
json(여기에 데이터넣기)이라는 뜻 data로 보냄

res.json() => json타입으로 리턴 받는다?

// set + expire
client.setex(redisKey, 3000, JSON.stringify(ress))


res.json() => json데이터를 보낸다라는 뜻
res.send와 res.json 중 res.json이 좋음
읽을때, json으로 읽겠다?라는 뜻

Body.json()은 비동기이며 PromiseJavaScript 객체로 해석 되는 객체를 반환 합니다. JSON.parse()동기식은 문자열을 구문 분석하고 결과로 반환되는 JavaScript 객체를 변경할 수 있습니다.