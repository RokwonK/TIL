---
title: IAM
author: 김록원
date: 2022.12.25
category: AWS
---

# IAM

> **💡 IAM이란?**  
Identity and Access Management의 약자.  
사용자를 생성하고 사용자들을 그룹으로 묶을 수도 있다.  
각 사용자들(혹은 그룹) 별로 권한을 설정할 수 있다.  

![출처 : https://choseongho93.tistory.com/263](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbF1cfE%2FbtqULB7GG48%2FF1wb4vPYgPe1DS2vg0TKJK%2Fimg.png)

<br />

### IAM을 사용하는 이유가 뭘까?
- AWS 계정을 사용하도록 허용하기 위해서.  
  - 허용을 위해서는 이들에게 권한을 부여 -> JSON 문서(Effect, Action, Resource 등)를 이용하여 지정 가능  
  - 이 정책들을 이용해 사용자들의 권한을 정의  
- 보안문제
  - 모든 사용자에게 모든 것을 혀용하지 않음 -> 서비스 실행에 큰 비용, 보안 문제등이 발생하기 때문에 **최소 권한 원칙**을 적용한다.(사용자/그룹 별 지정한 행동만 가능)  
  - 권한이 있는 자만 생성/접근/삭제 등을 허용
  - 이를 통해 조직 내 보안 수준을 높일 수 있음


<br />  


### IAM 정책  
사용자나 그룹에게 주는 권한이라고 볼 수 있다.  
정책은 JSON 문서로 작성할 수 있으며 문서 내 적성된 내용에 따라 리소스에 대한 접근수준이 정해진다.  

각 유저/그룹은 여러개의 정책을 가질 수 있다.

정책을 정의하는 문서내 구조를 살펴보자면
`Version` : 버전숫자 보통 '2012-10-17'  
`ID` :  


### 무조건 루트 사용자를 사용하는 경우