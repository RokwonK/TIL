---
title: Bastion Server 란
author: 김록원
date: 2022.09.23
category: Infrastructure
---

# Bastion Server란

> 외부와 내부(Private) 네트워크 사이에서 게이트 역할을 수행하는 호스트.

<br />

**언제 사용할까?**
- Piravte IP로만 접근이 허용된 서버에 접속하고 할때 경유지용도
- 접근 제어 기능(방화벽 역할 - 인증 처리 등)
- 프록시 서버로서의 역할 가능
- 외부에서 거쳐가는 경유지이므로 이곳에서 로그관리 가능
- 외부에서 내부 네트워크로 향햐는 공격의 1차 방어선


**단점**
- VPC 내 public 네트워크에서 private 네트워크로 접근이기때문에 SSH 프로토콜로 원격접속만 가능. HTTP 같은 프로토콜 사용 불가능