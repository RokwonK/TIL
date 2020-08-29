# iOS 개발 중 오류 모음

- ~ : this class is not key value coding-compliant for the key  
**Interface Builder 구성 에러**  
    - class 내부에 IB를 설정했다가 지웠을 때 생김  
    - 해결 => mainstroyboard에서 연결된 view에 오른클릭 Referencing Outlets에서 연결된 부분을 아예 지워줌

- http통신을 위해서 info.plist에 아래 내용 추가
```html
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```