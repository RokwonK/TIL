---
title: Terraform 기본 명령어, 파일/디렉터리
author: 김록원
date: 2022.12.31
category: Infrastructure
---

# Terraform - 기본 명령어, 파일/디렉터리  

간단하게 Terraform을 배우고 프로젝트에 바로 도입하였다가 프로젝트의 복잡도가 높아지면서 제대로 익히지 않았던 부분들이 발목을 잡았다. 한 번 얻어맞고보니 개념정리가 필요하다 싶어 정리해보았다.  

<br />  

### 기본명령어 - init, plan, apply  
`terraform init` provider가 정의되어 있는 위치에서 해당 명령어를 입력한다면 provider, module, state를 설정한다.(사용준비)  
👉 `.terraform` 디렉터리 생성  
👉 `.terraform.lock.hcl` 파일 생성  

`terraform plan` 이전 state와 비교하여 변경될 내역을 보여준다.  

`terraform apply` 현재 작성된 코드를 기준으로 인프라에 적용한다.  
👉 `terraform.tfstate` 파일 생성/업데이트
👉 `.terraform` 디렉터리 업데이트
👉 `.terraform.lock.hcl` 파일 업데이트

<br /><br />

## Terrform 파일/디렉터리
명령어를 사용하면 여러 파일/디렉터리가 생성, 업데이트가 되는 것을 볼 수 있다.  
이렇게 생성된 파일/디렉터리 들은 어떤 역할을 하는 것일까?  

<br />  

### .terraform 디렉터리  
의존되는 파일들이 다운로드(외부, 로컬까지도) 되어 저장되는 공간이다.  
내부적으로 .tfstate의 정보 또한 포함된다.  

<br />  

### .terraform.lock.hcl
**의존성 관련 잠금파일이다.** 의존되는 파일들에 대해 명시되어 있다.  
깃과 같은 버전관리 시스템에 `.terrform 디렉터리`를 업로드하지 않고 이 파일만 업로드한다. 이후 다른 PC/유저들은 `init` 명령어를 이용해서 이 파일에 명시되어 있는 의존 파일들을 다운로드 한다.  

<br />  

### terraform.tfstate  
Terrform의 상태파일로 `apply`를 통해 실제 인프라에 적용한 결과를 기억하는 파일이다.  
따로 설저하지 않는이상 로컬에서 관리되지만 인프라를 설계하는 인원이 많아질수록 이 파일을 원격으로 공유하면서 사용해야 할 이유가 명확하다.(각 로컬에서 관리할경우 모든 이들의 tfstate가 다르므로 인프라가 꼬일 확률이 높아진다.)  
또한 실제 인프라와 `.tfstate 파일`을 일치시키는 것이 중요하다. 