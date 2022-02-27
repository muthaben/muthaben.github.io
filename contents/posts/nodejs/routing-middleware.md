---
title: "라우팅과 미들웨어"
date: 2022-02-24
update: 2021-02-25
tags:
  - NodeJS
  - Express
---

> Express는 자체적인 최소한의 기능을 갖춘 라우팅 및 미들웨어 웹 프레임워크다.  
> Express 애플리케이션은 기본적으로 일련의 미들웨어 함수 호출이다.

## 라우팅

라우팅은 URL과 HTTP 메서드에 명시된 요청을 처리하는 메커니즘이다.

### 라우트 구조

```js
app.method(path, route handler)
```

- app: Express instance
- method: HTTP 요청 메서드
- path: route
- route handler: 요청 URL과 route가 일치할 때 실행되는 함수

### 라우트 핸들러(route handler)

```js
const express = require("express")
const app = express()

app.all("/", (req, res, next) => {
  console.log("모든 요청 메서드 처리")
  next()
})
app.get("/", (req, res) => {
  res.send("Hello World :)")
})
```

- 라우트 핸들러는 HTTP method 요청 경로와 route가 일치할 때 실행되는 콜백 함수다.
- 함수의 매개변수는 요청과 응답 객체이고, 문자열 "Hello World :)"를 리턴하는 `res.send()` 메서드를 호출한다.
- `app.all`메서드는 모든 요청 메서드를 처리한다.
- `next` 함수를 사용하면 후속 라우트 핸들러로 제어권을 넘긴다.

### 응답 메서드(response method)

- `res.send`: 다양한 유형의 응답 전송
- `res.json`: JSON 응답 전송
- `res.render`: view template 렌더링
- `res.redirect`: 요청 경로 재지정
- `res.end`: 응답 프로세스 종료

## 미들웨어

- HTTP 요청 상에서 동작하는 기능을 캡슐화하는 방법이다.
- 파이프라인(pipeline) 안에서 실행된다.  
  Express 앱에서는 `app.use`를 호출해 파이프라인에 미들웨어를 삽입한다.
- 컨베이어 벨트 상에서 **필요한 기능을 더하거나 불량품을 걸러내는 역할**에 비유할 수 있다.

### 자주 사용하는 미들웨어

- HTTP 요청의 url이나 method를 확인할 때
- POST 요청 등의 body(payload)를 분석할 때
- HTTP 요청/응답에 CORS 헤더를 추가할 때
- 요청 헤더에 담긴 유저의 인증 정보를 확인할 때

### 미들웨어 원칙

- `app.get`, `app.post` 등의 라우트 핸들러는 GET, POST 등의 특정 HTTP 동사만 처리하는 미들웨어다.  
  반면 미들웨어는 HTTP 동사 전체를 처리하는 라우트 핸들러라고 할 수 있다.
- 라우트 핸들러는 첫 번째 매개변수로 경로를 받는다. 모든 라우트에 일치하는 경로는 \*를 사용한다.  
  미들웨어 역시 첫 번째 매개변수로 경로를 받을 수 있지만 선택사항이다.
- 라우트 핸들러와 미들웨어는 2 ~ 4개의 매개변수가 있는 콜백함수다.  
  첫 번째와 두 번째 매개변수는 요청과 응답 객체이고 세 번째 매개변수는 next 함수다.  
  네 번째 매개변수가 있으면 그 함수는 에러를 처리하며, 에러 객체가 가장 첫 번째 매개변수가 된다.
- next 함수를 호출하지 않으면 파이프라인은 종료되고, 그 이후의 라우트 핸들러, 미들웨어는 호출되지 않는다.  
  이 상태에서 클라이언트에 응답(`res.send`, `res.json`,`res.render` 등)을 보내야 한다.
- next 함수를 호출했다면 클라이언트에 응답을 보내지 않는 것이 좋다.  
  일단 응답을 보내면 파이프라인 상 후속 미들웨어와 라우트 핸들러가 실행은 되지만, 이들이 보내는 응답은 무시된다.

---

<출처>

- [MDN: Express/Node 소개](https://developer.mozilla.org/ko/docs/Learn/Server-side/Express_Nodejs/Introduction)
- [Express 공식 문서: 미들웨어 사용](https://expressjs.com/ko/guide/using-middleware.html)
- [PoiemaWeb: Express Basics](https://poiemaweb.com/express-basics)
- 이선 브라운(한선용 역), 한 권으로 끝내는 Node & Express, 한빛미디어(2021)
