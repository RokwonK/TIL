---
title: IAM
author: 김록원
date: 2022.12.25
category: AWS
---

# AWS IAM 서비스
AWS를 처음 사용할때 콘솔에서 계정/비밀번호를 이용하여 로그인한다. 일반적인 서비스에서의 로그인방법과 유사하다.  
하지만 AWS는 인프라를 구성할 수 있는 서비스이다. 조직에서 인프라에 접근할 수 있는 사람은 한 명이 아니다. 그렇다면 다수의 인원이 하나의 계정을 사용할까? 또는 막 합류한 개발자에게 기존 서비스를 삭제하고 생성할 수 있는 권한이 있다면 문제가 생기지 않을까? 이러한 **보안문제에 대한 해답을 제시하는 서비스가 바로 AWS IAM**이다.  

<br />

> **💡 IAM이란?**  
Identity and Access Management의 약자.  
사용자/그룹을 생성할 수 있으며 리소스에 대한 접근권한을 부여할 수 있다.  
여러 사용자에게 AWS 계정에 대한 접근을 허용할 수 있으며 내/외부적인 보안문제를 처리할 수 있다.

아래의 그림은 하나의 계정에 대해 IAM을 사용하여 사용자들을 어떤식으로 구성할 수 있는지를 보여준다.

![IAM](https://user-images.githubusercontent.com/52196792/209523402-82eeb909-f566-4096-8932-bd5361cecb3a.png)


<br />

### IAM을 사용하는 이유?
위에서 이미 얘기했지만 더 자세히 살펴보자
- **AWS 계정을 사용하도록 허용하기 위해서**
  - 하나의 계정을 여러 사용자들이 사용할 수 있음
  - 사용자/그룹 별로 정책을 이용하여 접근권한 수준을 정할 수 있음
- **내/외부적인 보안문제 해결**
  - 정책을 이용하여 사용자별 권한수준 정의(AWS는 **최소 권한 원칙**준수를 지향한다.)
  - IAM에서 제공하는 보안기술들을 이용하여 해킹의 위험성 낮출 수 있음

<br /><hr><br />

## IAM의 기능
IAM에는 여러 기능이 존재한다.  
IAM에 존재하는 기능들은 유저/그룹/리소스 에게 권한을 설정, 계정/유저 단위의 보안과 연관된 것들이다.  
AWS를 운용하기 위해 필요한 기능들을 살펴보자  

<br />

### IAM Policy
IAM Policy(정책)은 **`권한에 대한 명세`** 라고 볼 수 있다.  
**유저/그룹/리소스 들은 이 정책을 부여받아 특정 리소스에 대한 접근권한**을 가질 수 있다.  

정책은 JSON 문서이며 문서 내 작성된 내용에 따라 접근수준이 정해진다. 각 유저/그룹은 여러개의 정책을 가질 수 있다.  

JSON 문서 구조를 살펴보자면 
|Key |설명|
|:---|:---|
| `Version`   | 문서 버전명. 보통 '2012-10-17' |
| `ID`        | (Optional) 정책에 대한 ID     |
| `Statement` | 실제 권한에 대한 정보가 들어감     |


`Statement`내에 들어가는 값들 또한 형식이 정해져 있다.
|Key |설명|
|:---|:---|
| `Sid`       | (Optional) 해당 문장의 ID로 문장의 식별자 |
| `Effect`    | 해당 정책을 Allow 할 것인지 Deny 할것인지를 정의 |
| `Principal` | 정책이 적용될 사용자, 계정 혹은 역할로 구성된다 |
| `Action`    | Effect에 기반하여 허용/거부 되는 API 호출 목록 |
| `Resource`  | 적용될 Action의 리소스의 목록 |
| `Condition` | (optional) 이 정책이 언제 적용될지를 결정함 |

<br />
 
실제 정책에 대한 예시를 보자
```json
{
  "Version": "2012-10-17",
  "Id": "S3-Permissions",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": ["arn:aws:iam::1234566789012:root"]
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": ["arn:aws:s3:::testbucket/*"]
    }
  ]
}
```  

위 코드에 대한 설명은 아래와 같다  
```txt
Principal : root 사용자에게  
Resource  : testbucket 이름을 가진 s3 서비스의 모든경로에 대해
Action    : Get과, Put을 할수 있는 권한을  
Effect    : 허용하겠다
```

위와 같은 형식을 이용하여 권한에 대한 명세. 즉, 정책을 정의할 수 있다.

<br />  

### IAM User/Group  
IAM User(사용자)은 실제로 단 한 명의 사용자를 의미하는 것이다.  
IAM Group은 다수의 IAM 사용자를 포함할 수 있다. 그룹을 이용하여 정책을 다수의 사용자에게 매핑 시킬 수 있다. 

![IAM User](https://user-images.githubusercontent.com/52196792/209518566-f5285671-8f97-4713-ace5-c8a4dd50d93c.png)  

![IAM User](https://user-images.githubusercontent.com/52196792/209518730-b70f783c-8e20-43da-a117-f6f7a3ac5252.png)  

<br />

### IAM Role
IAM Role(역할)은 특정 개체(User, AWS 서비스, 다른 계정 등)로의 접근권한을 가진다.  
Role은 Policy을 가진다는 것에서 IAM User와 비슷하게 보이지만 영원히 가지고 있는 권한이 아닌 `임시 보안 자격 증명`이다.  
보통 Role은 모자로 비유가 된다. 언제든 썼다 벗을 수 있다는 것이다.  
또한 Role은 사용자뿐만 아니라 서비스에게도 부여할 수 있다.(AWS 내 서비스에게 권한 부여한다는 뜻)  

임시 자격 증명이기때문에 보다 안전하다는 장점이 있다.

![IAM User](https://user-images.githubusercontent.com/52196792/209520973-0a0e3f01-9779-4a08-b050-6ad2c029e69e.png)  

<br />  

### IAM 보안(Password, MFA)
사용자의 정보가 침해당하지 않도록 보호하기 위해 두가지 방어 메커니즘이 있다.  
- 비밀번호 정책 - 비밀번호가 강할수록 보안이 높음
  - 최소 글자 수, 특정 문자들을 요구
  - 유저 비밀번호 변경을 허용 또는 금지
  - 일정 기간지나면 비밀번호 만료시키기(이전 비밀번호와 동일하게 변경 불가능)
  - AWS IAM -> 좌측 메뉴중 Account settings에서 비밀번호 정책 확인가능  
- MFA(Multi Factor Authentication)
  - MFA = 비밀번호와 보안장치를 함께 이용하는 것  (더 안전해진다)
  - 해킹당해 비밀번호가 누출되어도 보안(물리적)장치가 필요하므로 계정에 침투 불가능
  - `가상 MFA 장치` = Google Authenticator(phone only), Authy(multi device)
  - `Universal 2nd Factor(U2F) Security Key` = 물리적 장치임 YubiKey(하나의 키로 여러 유저의 정보를 가질 수 있음)
  - `Hardware Key Fob MFA Device` = Gemalto 회사의 제품, AWS GovCloud(미국 정부가 제공)
  - 상단 계정클릭 -> Security Credential -> MFA 확성화

<br />

### IAM Security Tools
- IAM Credentials Report(계정 레벨에서 확인)  
  - 계정을 언제 사용했는지, 비밀번호를 언제 변경했는지 등
- Access Advisor(유저 레벨에서 확인)  
  - 액세스 로깅을 확인하고 해당 유저는 이정도의 권한만 있으면 되겠다 판별할 수 있음
  - 이를 이용해 최소 권한 원칙을 적용할 수 있음

![IAM Credentials Report](https://user-images.githubusercontent.com/52196792/209522230-7dd184d4-4bf1-4713-8f5f-7366205d85ba.png)  

![Access Advisor](https://user-images.githubusercontent.com/52196792/209522463-c48e235a-cb21-4b16-9978-bffafa31bb13.png)

<br />  

### IAM 제대로 사용하기
- 루트 계정으로는 접근하지 말자(필요하다면 MFA를 이용하여 보안수준을 높인 후 이용하자)
- 하나의 유저는 한 명의 실제 사용자를 의미하도록 하자
- 그룹을 이용하여 그룹 수준에서 보안을 관리할 수 있도록 하자
- 비밀번호 정책을 사용하자

<br />

### IAM 키워드 요약
- Users : 실제 사용자와 IAM 사용자를 매핑
- Groups : 사용자만을 포함한다.(다른 그룹을 포함할 수는 없다)
- Policies : JSON 문서로 만들어진 권한 정책으로 유저나 그룹에게 부여할 수 있다.(접근 권한에 대한 명세)
- Roles : 사용자가 아닌 AWS 서비스에게 권한 정책을 부여할 수 있다. (서비스에게 다른 서비스에 대한 권한을 부여하는 기능)
- Security : MFA + Password Policy
- Security Tools : IAM Credential Reports & IAM Access Advisor