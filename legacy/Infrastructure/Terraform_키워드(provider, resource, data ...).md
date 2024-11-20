---
title: Terraform 키워드
author: 김록원
date: 2022.12.31
category: Infrastructure
---

# Terraform - 키워드(provider, resource, data, variable, local)  

Terrform은 HCL(Hashicorp Configuration Language)라는 설정언어를 사용한다.  
그에 따라 내부적으로 사용하는 여러 키워드들이 존재하는데 핵심적인 키워드들을 정리해보았다.  

<br />  

## 계정, 서비스와 관련된 키워드  
Terraform을 사용하는데 거의 필수적인 키워드로는 `terraform`, `provider`, `resource`, `data`가 있다. 작은 인프라를 구성하더라도 반드시 필요한 키워드들이다.  

<br />  

### terraform
`terraform` 키워드로 선언된 블럭은 테라폼 자체에 대한 설정을 작성하는 블럭이다.  
예를 들어 `provider`에 대한 버전, tfstate 파일의 원격저장소 설정 등을 이 블럭에 작성한다. 아래의 예시는 provider 중 사용할 aws의 버전을 구체적으로 명시한다.  
[Terraform Settings 공식문서](https://developer.hashicorp.com/terraform/language/settings)  

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "4.33.0"
    }
  }
}
```

<br />  

### provider
**Terraform과 외부 서비스를 연결해주는 기능을 한다.**  
알다시피 Terraform은 크로스플랫폼 IaC도구이다. AWS뿐만 아니라 Azure, Google Cloud Platform 등 여러 클라우드 서비스와 연결하여 사용할 수 있다.  
그에 따라 Terraform은 사용할 provider를 반드시 선언해야한다. 다수의 클라우드 서비스를 이용할 경우 여러개의 provider를 선언할 수 있다. 이후에 살펴볼 `resource`나 `data`에서는 각각 선언된 provider 중 선택하여 설정할 수 있다.  
아래의 링크에서 사용가능한 클라우드 서비스를 확인할 수 있다.
[Terraform Providers](https://registry.terraform.io/browse/providers)  

아래와 같이 `CLOUDNAME` 공간에 사용할 provider를 작성한다.  
```hcl
provider CLOUDNAME {
  ...
}
```  

aws를 이용하고자 한다면 아래와 같이 작성할 수 있다.  
연결할 region과 연결될 IAM User를 작성해 주었다.  
이 외에도 여러가지 Key를 작성할 수 있다. 첨부한 링크(공식문서)에서 더 알아볼 수 있다.
```hcl
provider "aws" {
  region                   = "ap-northeast-2"
  shared_config_files      = ["~/.aws/config"]
  shared_credentials_files = ["~/.aws/credentials"]
  profile                  = "thepool"
}
```
[Terraform Provider Configuration Docs](https://developer.hashicorp.com/terraform/language/providers/configuration)  
[Terraform Provider API Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

<br />  

### resource  
`resource`는 Terraform에서 가장 중요하며 가장 자주사용하는 요소이다.  
하나의 `resource` 블럭은 하나의 인프라 구성요소와 같다. ec2나 subnet 혹은 s3 등 하나의 `resource`로 표현되어진다.  

`resource`는 다음과 같은 형식으로 작성한다. `TYPE`은 Terraform에 정의되어 있는 타입의 이름이고 `NAME`은 개발자가 붙이는 이름이다.  
```hcl
resource TYPE NAME {
  ...
}
```  

예를 들어, aws의 vpc를 만들고자 한다면 아래와 같이 작성할 수 있다.  
`cidr_bloc`이나 `tags`와 같은 Key들은 aws_vpc를 만들때 필수적이거나 선택적인 설정항목이다.   
```hcl  
resource "aws_vpc" "my_vpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "the-pool-vpc"
  }
}
```  

aws의 여러 resource가 정리된 공식문서이다.
[Terraform API Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)  
[Terraform Resource Configuration Docs](https://developer.hashicorp.com/terraform/language/resources/syntax)  

<br />  

### data  
Terraform 외부에 정의되어 있는 정보를 가져와서 사용할 수 있게 해주는 키워드이다.  
각 `provider`들은 `resource` 뿐만 아니라 `data`와 관련된 서비스들도 지원해준다.  

`resource`와 마찬가지로 `TYPE`은 Terraform에 정의되어 있는 타입의 이름이고 `NAME`은 개발자가 붙이는 이름이다.
```hcl
data TYPE NAME {
  ...
}
```  

다음은 aws IAM Policy를 가져와 사용할 수 있도록 하는 코드이다.  
```hcl
data "aws_iam_policy_document" "s3_policy" {
  statement {
    principals {
      type        = "*"
      identifiers = ["*"]
    }

    actions = [
      "s3:*",
    ]

    effect = "Allow"

    resources = [
      "${aws_s3_bucket.s3.arn}",
    ]
  }
}
```  

[Terraform Data Source Configuration Docs](https://developer.hashicorp.com/terraform/language/data-sources)


<br /><hr><br />

## 변수, 출력과 관련된 키워드  
Terraform으로 인프라를 구축하다보면 필연적으로 코드, 값이 중복되는 일이 허다하다.  
이를 위해 Terraform에서는 변수 및 결과를 출력해주는 방법을 마련해주었다.  
아래에서 설명하는 `local`, `var`, `output` 이외에도 모듈화에 필요한 `module` 키워드도 있지만 매우 중요한 개념이기에 따로 해당 키워드는 따로 정리하였다.  

<br />  

### variable  
`variable`을 사용하면 여러 파일에서 같은 모듈을 공유할 수 있어 재사용성이 매우 뛰어나다. 기본값을 설정할 수 있으면 **개발자로 부터 입력을 받을 수 있는 변수**이다.  
뿐만 아니라 입력값에 조건을 달 수도 있다.  

다음과 같은 구조를 가지며 `NAME`은 변수의 이름이다. 내부적으로 들어가는 type은 변수의 타입이며 default는 기본설정 값이다.
type으로는 string, number, bool 뿐만아니라, list(TYPE), set, map, object, tuple 이 들어갈 수 있다.  
```hcl  
variable NAME {
  type = TYPE
  defalut = VALUE
}
```  

입력변수를 사용할때는 `var`이라는 키워드를 이용하여 접근할 수 있다.  
```hcl
variable "vpc_name" {
  type    = string
  default = "test-vpc"
}

resource "aws_vpc" "the_pool_vpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = var.vpc_name
  }
}
```  

이렇게 선언된 변수는 선언된 모듈내에서만 액세스가 가능하다.  
`variables`를 입력하는 방법은 여러가지가 있다. CLI로 직접 입력하는 방법, `.tfvars`내에 작성하고 사용하는 방법, 환경변수로 지정 후 사용하는 방법 등이다.  
자세한 내용은 아래 공식문서에서 확인할 수 있다.  

[Terraform Input Variables Docs](https://developer.hashicorp.com/terraform/language/values/variables)  

<br />  

### locals  
`locals`은 지역변수 선언 키워드이다. 현재 파일에서만 사용이 가능하다. 특정값들을 연산(merge, concat, max)하여 하나의 변수로 만들때 주로 사용한다.  

`NAME`에 변수명이 들어가고 `VALUE`에는 값이 들어간다.  
```hcl
locals {
  NAME = VALUE
}
```  

`locals`에 정의된 값은 `local.NAME`으로 사용가능하다.  
```hcl
locals {
  # Common tags to be assigned to all resources
  common_tags = {
    Service = local.service_name
    Owner   = local.owner
  }
}

resource "aws_instance" "example" {
  # ...
  tags = local.common_tags
}
```

[Terraform Local Values Docs](https://developer.hashicorp.com/terraform/language/values/locals)


<br />  

### output  
`output`은 인프라에 대한 정보를 명령줄에 노출(숨기기 가능)시킬 수 있으며 자식 모듈의  `output`값에 접근하여 사용할 수 있다.  

`NAME`에는 지정할 이름이 들어가고 `VALUE`에는 출력할 결과값을 작성할 수 있다.  
```hcl  
output NAME {
  value = VALUE
} 
```  

다음은 생성된 ECR `resource`의 url을 출력한다.  
```hcl
resource "aws_ecr_repository" "ecr" {
  name                 = "test_ecr"
  image_tag_mutability = "MUTABLE"
  image_scanning_configuration {
    scan_on_push = true
  }
}

output "ecr_registry_url" {
  value = aws_ecr_repository.ecr.repository_url
}
```  

[Terraform Output Values Docs](https://developer.hashicorp.com/terraform/language/values/outputs)  
