# Terraform 명령어 등 정리

간단하게 Terraform을 배우고 프로젝트에 바로 도입하였다. 프로젝트가 성장하면서 복잡해짐에 따라 개념을 제대로 익히지 않았던 것이 발목을 잡았다. 낑낑 앓다가 정리를 먼저하고 진행하는게 더 좋다고 생각해 개념들을 제대로 정리해보기로 했다.  

### init, plan, apply
`terraform init` provider가 정의되어 있는 위치에서 해당 명령어를 입력한다면 provider, module, state를 설정한다.(시작 준비)
👉 `.terraform` 디렉터리 생성  
👉 `.terraform.lock.hcl` 파일 생성  

`terraform plan` 이전 state와 비교하여 변경될 내역을 보여준다.  

`terraform apply` 현재 작성된 코드를 기준으로 인프라에 적용한다.  
👉 `terraform.tfstate` 파일 생성/업데이트
👉 `.terraform` 디렉터리 업데이트
👉 `.terraform.lock.hcl` 파일 업데이트

<br /><br />

## 생성 파일,디렉터리
명령어를 사용하면 여러 파일/디렉터리가 생성, 업데이트가 되는 것을 볼 수 있다.  
이것들은 어떤 역할을 하는 것일까?  

<br />  

### terraform.tfstate

### terraform.tfstate.backup

### .terraform.lock.hcl

### .terraform 디렉터리  



<br /><hr><br />  


## module
인프라의 규모가 커질 경우 코드가 중복되어 쌓일 수가 있다.  
이때 리소스를 각 파일별로 나누고 `module`을 이용한다면 각 리소스들을 재사용하여 사용할 수 있다. (리소스 모듈화 및 `module` 키워드 이용)  
`module`을 이용한다면 관련있는 요소(리소스)끼리 모아 하나의 패키지를 만들 수 있다.  

모듈화 및 `module`을 이용하면 아래와 같은 이점을 얻을 수 있다.  
1. 캡슐화(요소들을 합쳐 새로운 요소를 만들어 낼 수 있다).  
2. 재사용성(모듈이기에 필요한 곳에서 참조하여 사용가능하다.)
3. 일관성(하나의 모듈을 재사용함으로써 일관된 구조를 가져갈 수 있다.)  

### 모듈로 사용할 수 있는 것들  

### 모듈 입력값  

### 모듈 주의사항 - 파일경로  

<br /><hr><br />  

## backend 


<br /><hr><br />  

## Terraform에서 변수표현  

### local

### var

### output

### data  


<br /><hr><br />  

## Terraform ++

### import


<br />  

## 참고자료  
- [테라폼 모듈이란?](https://j-dev.tistory.com/entry/%ED%85%8C%EB%9D%BC%ED%8F%BCTerraformChapter-4-1-%ED%85%8C%EB%9D%BC%ED%8F%BC-%EB%AA%A8%EB%93%88%EB%A1%9C-%EC%9E%AC%EC%82%AC%EC%9A%A9-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%9D%B8%ED%94%84%EB%9D%BC-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0%EB%AA%A8%EB%93%88%EC%9D%B4%EB%9E%80)
- [[Terraform] local, variable, output, data 차이](https://kim-dragon.tistory.com/219)
- [[Terraform] 테라폼(terraform)state 관리(local,terraform cloud,s3)](https://2ham-s.tistory.com/405)