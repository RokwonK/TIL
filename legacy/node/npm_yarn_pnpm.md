---
title: npm, yarn, pnpm
author: 김록원
date: 2022.09.26
category: node
---

node.js의 패키지 관리자들

### npm
- metadata 작성 `package.json`
- 의존 라이브러리 추가, 버전업, 삭제
- 스크립트 실행 및 단축기 설정
- 보안 검사

### yarn
`Yet Another Resource Negotiator`

- npm을 기반으로 설계(npm과 호환)
- React Project 진행하는 도중 facebook에서 개발
- npm의 설치 속도를 높임(라이브러리 설치 병렬화)
- npm보다 보안적인 측면에서 더 안전함.
    - npm은 패키지에 포함된 다른 패키지 코드를 실행 → 보안 취약
    - yarn은 `yarn.lock` or `pacakge.json`에 선언되 파일만 설치
    - 최근 npm이 업데이트하며 보안 업데이트도 크게 향상됨
- npx 사용불가
- `.yarn/` 밑에 `cache/` 가 있어 설치 시 속도 향상
- `package-lock.json` 대신 `yarn.lock` 파일 생성

### pnpm
여러 레포지토리를 운영시 효율적인 디스크 관리를 해줌. **라이브러리 공통 관리가 핵심포인트**

- 프로젝트(레포지토리) 간에 사용되는 의존 라이브러리들이 각 레포마다 중복 저장이되는 것을 해결
- 설치되는 라이브러리들을 symlink(주소참조)를 이용하여 중앙 집중 관리(home 폴더의 `.pnpm-store` 에서 관리)
- npm 사용가능