---
title: Terraform 기본 명령어, 파일/디렉터리
author: 김록원
date: 2023.01.02
category: Infrastructure
---


# Terraform - module 
인프라의 규모가 커질 경우 코드가 중복되어 쌓일 수가 있다.  
이때 리소스를 모듈별(하나의 폴더)로 나누고 `module` 키워드를 사용한다면 각 리소스들을 재사용하여 사용할 수 있다.(리소스 모듈화 및 `module` 키워드 이용)  
`module`을 이용한다면 관련있는 모듈끼리 모아 하나의 패키지를 만들 수 있다.  

`module`을 이용하면 아래와 같은 이점을 얻을 수 있다.  
1. 캡슐화(모듈을 이용해 새로운 모듈을 만들어 낼 수 있다).  
2. 재사용성(하나의 모듈을 필요한 여러곳에서 참조하여 사용가능 하다.)
3. 일관성(모듈을 사용하는 방법을 통일시킬 수 있다.)  


`module`은 아래와 같은 형태로 사용가능하다. `PATH`에는 디렉터리 경로를 나타낸다.
```hcl
module NAME {
  source = "PATH"
  ...
}
```  

<br />  

### 모듈의 특징  
- 모듈(하나의 .tf파일)에 따라 한개이상의 `resource`들을 포함한다.  
- Root Module
  - Terraform 명령어를 수행하는 디렉터리에 있는 파일들로 구성된 module
- Child Module
  - 다른 module(root 디렉터리, 혹은 하위 디렉터리)에서 호출하여 사용되는 module
  - 이 모듈들은 재사용될 수 있고 호출하는 곳에 config 값을 전달할 수 있다.  
- Published Module
  - Terraform Registry, Github와 같은 저장소로부터 받아 사용할 수 있다.

<br />  

### 모듈의 `source`로 사용할 수 있는 것들  
- Local path  
- Git, Github  
- S3  
- Terraform Registry  
- etc(Google Cloud Storage)..

<br />  

## 참고자료  
- [Terraform 공식문서](https://developer.hashicorp.com/terraform/language/modules)
- [테라폼 모듈이란?](https://j-dev.tistory.com/entry/%ED%85%8C%EB%9D%BC%ED%8F%BCTerraformChapter-4-1-%ED%85%8C%EB%9D%BC%ED%8F%BC-%EB%AA%A8%EB%93%88%EB%A1%9C-%EC%9E%AC%EC%82%AC%EC%9A%A9-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%9D%B8%ED%94%84%EB%9D%BC-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0%EB%AA%A8%EB%93%88%EC%9D%B4%EB%9E%80)  
- [Terraform을 기반한 AWS 기반 대규모 마이크로서비스 인프라 운영 노하우 - 이용욱(삼성전자)](https://www.youtube.com/watch?v=9PTdO7DM6XQ&t=1189s)  