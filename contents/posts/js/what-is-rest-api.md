---
title: "REST API"
date: 2022-02-10
update: 2021-02-10
tags:
  - JavaScript
  - HTTP
---

REST(Representational State Transfer) API는 **REST 규칙을 준수**하여 설계한 `API`다.

📝 API(Application Programming Interface)

- 프로그램 간의 상호작용을 도와주는 **매개체**
- 다른 서비스에 리소스를 요청하고 응답을 받기 위해 정의된 **명세**

## REST API의 구성

REST API는 자체 표현 구조(Self-descriptiveness)로 구성되어 **그 자체로 요청을 파악**할 수 있다.

| 구성 요소       | 내용                    | 표현 방법             |
| :-------------- | :---------------------- | --------------------- |
| Resource        | 자원                    | HTTP URI              |
| Verb            | 자원에 대한 행위        | HTTP Method           |
| Representations | 자원에 대한 행위의 내용 | HTTP Message Pay Load |

## REST의 규칙

### 1. URI는 리소스 자체를 표현하는데 집중한다.

리소스의 이름은 동사보다는 **명사**를 사용한다(리소스에 대한 **행위의 명사형이 아님**).

```
// 기존 회원 정보 조회할 때

# 👎
GET /inquery
GET /users/show/1

# 👍
GET /users/1

// 신규 회원 등록할 때

# 👎
GET /users/register/2

# 👍
POST /users/2
```

### 2. 리소스에 대한 행위는 HTTP 메서드로 표현한다.

역할에 맞는 메서드를 사용하여 **CRUD**를 구현한다.

| 메서드 | 역할                              | 페이 로드 |
| :----- | :-------------------------------- | :-------- |
| GET    | 모든/특정 리소스 조회(**R**ead)   | X         |
| POST   | 리소스 생성(**C**reate)           | O         |
| PUT    | 리소스 전체 교체(**U**pdate)      | O         |
| PATCH  | 리소스 일부 수정(**U**pdate)      | O         |
| DELETE | 모든/특정 리소스 삭제(**D**elete) | X         |

### 3. HTTP 응답 상태 코드

REST API에서 URI 설계만큼 중요한 것은 **정확한 응답 상태 코드**를 전달하는 것이다.

| 상태 코드  | 의미                                                              |
| :--------- | :---------------------------------------------------------------- |
| 성공       |                                                                   |
| 200        | 클라이언트의 요청 정상 처리                                       |
| 201        | 리소스 생성 성공                                                  |
| 요청 오류  |                                                                   |
| 400        | 잘못된 요청(클라이언트의 요청이 부적절한 경우)                    |
| 401        | 인증 실패                                                         |
| 402        | 결제 필요                                                         |
| 403        | 인가 실패                                                         |
| 404        | 요청한 리소스를 찾을 수 없음                                      |
| 405        | 허용되지 않은 메서드                                              | \  |
| 서버 오류  |                                                                   |
| 500        | 내부 서버 오류(요청 처리 불가)                                    |
| 501        | 서버에 요청을 구현할 기능이 없음                                  |
| 리다이렉션 |                                                                   |
| 301        | 요청한 리소스의 URI 변경(응답 시 Location header에 변경 URI 표시) |

<출처>

- https://ko.wikipedia.org/wiki/API
- https://poiemaweb.com/js-rest-api
- https://meetup.toast.com/posts/92
- https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C
