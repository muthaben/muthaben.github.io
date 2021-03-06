---
title: "타입스크립트 컴파일러"
date: 2022-02-25
update: 2021-02-25
tags:
  - TypeScript
---

## 컴파일(compile) 과정

- 컴파일러는 프로그래머가 작성한 `소스 코드`를 파싱하여 `추상 구문 트리(AST)`라는 자료구조로 변환한다.
  - `소스 코드`: 컴퓨터 프로그램을 사람이 읽을 수 있는 프로그래밍 언어로 기술한 텍스트 파일
  - `추상 구문 트리`: 컴파일러의 구문 분석 단계의 결과물로서 프로그램 코드의 구조를 표현. 공백, 주석
    등을 무시
  - 타입스크립트 컴파일러는 AST 생성 전에 타입을 확인한다.
- 컴파일러는 AST를 다시 `바이트 코드(bytecode)`로 변환한다.  
  런타임에 바이트 코드를 입력해 평가하고 결과를 얻을 수 있다.
  - `바이트 코드`: 특정 하드웨어가 아닌 가상 컴퓨터 상의 실행 프로그램을 위한 이진 표현법
- 컴파일은 일반적으로 소스 코드를 바이트 코드로 변환하는 작업이다.
  - 타입스크립트 컴파일러는 타입스크립트 파일을 자바스크립트 파일로 변환하므로  
    컴파일 보다는 트랜스파일링(transpiling)이 보다 적절한 표현이다.

## 타입스크립트 컴파일 과정

<TSC가 수행>

1. 타입스크립트 소스 ➡️ 타입스크립트 AST
2. 타입 검사기(typechecker)가 AST 확인
3. 타입스크립트 AST ➡️ 자바스크립트 소스

<자바스크립트 런타임(브라우저, NodeJS)이 실행>  
4. 자바스크립트 소스 ➡️ 자바스크립트 AST  
5. AST ➡️ 바이트 코드  
6. 런타임이 바이트 코드 평가

💡 타입 정보 확인

- 과정 1 ~ 2에서는 소스 코드에 사용된 타입을 이용하지만 과정 3에서는 이용하지 않는다.  
  즉, TSC가 타입스크립트 코드를 자바스크립트 코드로 컴파일할 때는 개발자가 작성한 타입을 확인하지 않는다.  
  타입 정보는 최종 프로그램에 아무런 영향을 주지 않고 단지 타입 확인에서만 이용된다.

### 타입스크립트와 자바스크립트의 타입 시스템

| 타입 시스템 기능    | 타입스크립트   | 자바스크립트        |
| :------------------ | :------------- | :------------------ |
| 타입 결정 방식      | 정적           | 동적                |
| 타입 자동 변환 여부 | O              | X(대부분)           |
| 타입 확인 시점      | 런타임         | 컴파일 타임         |
| 에러 검출 시점      | 런타임(대부분) | 컴파일 타임(대부분) |

---

<출처>

- [위키백과: 소스 코드](https://ko.wikipedia.org/wiki/%EC%86%8C%EC%8A%A4_%EC%BD%94%EB%93%9C)
- [위키백과: 추상 구문 트리](https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%EA%B5%AC%EB%AC%B8_%ED%8A%B8%EB%A6%AC)
- [위키백과: 바이트 코드](https://ko.wikipedia.org/wiki/%EB%B0%94%EC%9D%B4%ED%8A%B8%EC%BD%94%EB%93%9C)
- [PoiemaWeb: 타입스크립트 소개와 개발 환경 구축](https://poiemaweb.com/typescript-introduction)
- 보리스 체르니(우정은 역), 타입스크립트 프로그래밍, 프로그래밍 인사이트(2021)
