---
title: "[Review] 자바스크립트는 왜 프로토타입을 선택했을까"
date: 2022-01-31
update: 2021-02-15
tags:
  - JavaScript
  - Review
series: Review
---

<a href="https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42" target="_blank">
  <img src="./prototype-in-js.png" alt="prototype-in-js">
</a>

<h6 align=center>사진 출처: 임성묵, 자바스크립트는 왜 프로토타입을 선택했을까</h6>

---

비트겐슈타인의 사진이 인상적이었던 임성묵님의 블로그 글을 읽어 내려가면서 그 동안 프로토타입 기반 상속과 this 개념을 마주할 때 느꼈던 답답함을 해소할 수 있었다. '결국 암기를 해야하나보다...' 하면서 억지로 삼켜 넘기려던 개념을 좀 더 깊이 이해할 수 있겠다는 자신감도 얻었다.  
철학으로 프로그래밍을 풀어내는 과정이 무척 흥미로웠고, 프로그래밍도 인간의 관념을 다루며 그것을 현실에 구현할 때 사용하는 언어가 우리 일상의 언어와 닮아있다는 사실이 새삼 신기했다.

---

---

<본문 내용>

### 플라톤과 이데아, 그리고 클래스 기반 객체지향 프로그래밍

- 프로토타입 기반 OOP는 class 기반의 OOP와 완전히 상반되는 방식이다. 객체를 바라보는 개념 자체가 다르다. 특히 **문맥(context)**을 강조하는 철학적 근거에서 태어났기에 렉시컬 스코프에 의한 호이스팅과 실행 문맥에 의한 복잡한 this가 필연적으로 발생할 수밖에 없다.

> 눈앞에 실제로, 구체적으로 존재하는 사물이 있다면 반드시 그것의 **본질**이 존재한다. — 플라톤

- 이러한 사고방식이 프로그래밍에도 자연스럽게 녹아들어 생긴 언어가 "**클래스** 기반 객체지향 프로그래밍 언어"다. 대표적으로 Java, C#이 있다.

### 분류(classification)

- 플라톤의 **이데아** 이론은 아리스토텔레스에 의해 '**분류(classification)**'라는 개념으로 정립된다.
  - 개체의 속성이 동일한 경우 개체 그룹이 같은 그룹에 속한다.  
    범주는 정의와 구별의 합이다.
  - 이는 전통적인 클래스 기반 객체 지향 프로그래밍의 **아이디어-일반화**와 일치한다. 속성은 클래스의 프로퍼티가 되고, 프로퍼티가 유사한 객체가 있다면 일반화 과정을 통해 클래스로 추상화된다.

### 프로토타입(prototype)

- 프로토타입이란 개념은 **분류(classification)이론을 정면으로 반박**하여 나왔다.
  > - 공유 속성의 관점에서 정의하기 어려운 개념이 있다(사실상 올바른 분류란 없다).
  > - 세계에 미리 내재되어서 대상과 언어를 완전히 규정하는 어떤 언어란 존재하지 않는다.
  > - 표현은 삶의 **흐름** 속에서만 **의미**를 갖는다.  
  >   — 비트겐슈타인

### 의미사용이론(the use theory of meaning)

- 단어의 '진정한 본래의 의미'란 존재하지 않고 '**상황과 맥락**'에 의해서 결정된다.
  > - 언어와 세계는 본질적으로 동일하여 1:1 대응 관계에 있는 것이 아니다.
  > - 하나의 언어에 반드시 하나의 존재가 대응하지는 않는다.
  > - 언어란 사용에 의해서 의미가 결정된다.
  > - 언어는 초월적, 관념적, 선험적으로 미리 주어져 있는 것이 아니다.
  > - 언어의 의미란 인간이 언어놀이로 만들어 낸 것이다.  
  >   — [비트겐슈타인 후기 사상: 용도의미론(the use theort of meaning)](https://imnt.tistory.com/207)

### 가족 유사성(family resemblance)

- 비트겐슈타인은 인간이 현실에서 실제로 대상을 분류할 때 속성(전통적인 분류에서의 기준)이 아닌 가족 유사성을 통해 분류하게 된다고 이야기한다.
- 모든 가족 구성원에게 적용되는 공통된 특성(속성)은 있을 수 없다.  
  ... 우리는 전형적인 특징을 통해 가족을 분류한다.  
  이 이론은 프로토타입 이론의 근거가 된다.

### Rosch의 프로토타입 이론(prototype theory)

- 비트겐슈타인의 의미사용이론, 가족 유사성은 1970년경 철학자 Eleanor Rosch에 의해 프로토타입 이론으로 정리된다.
  - 인간은 '등급이 매겨진 (개념) 구조(graded structure)'를 가진다.
  - 인간은 사물을 분류할 때 자연스럽게 가장 유사성이 높은 것 순서대로 등급을 매긴다.
  - 이렇게 분류했을 때 가장 높은 등급을 가진 것이 바로 **원형(prototype)**이다.
- 객체는 '정의'로부터 분류되는 것이 아니라 가장 좋은 보기(prototype, examplar)로부터 범주화된다.
  - 현실에 존재하는 것 중 가장 좋은 본보기를 원형으로 선택한다.
  - 문맥(컨텍스트)에 따라 '범주', 즉 '의미'가 달라진다.

### 프로토타입 기반 객체지향 프로그래밍

- 프로토타입 언어에서는 '분류'를 우선하지 않는다. 생성된 객체 위주로 유사성을 정의한다.
- 어휘, 쓰임새는 맥락(context)에 의해 평가된다.
  - 실행 컨텍스트, 스코프 체인이 여기서 파생되었다.
  - 클로저, this, 호이스팅 등이 프로토타입의 '**맥락**'을 표현하기 위한 것이다.

#### 자바스크립트 — 어휘적 범위(lexical scope)

- 변수의 의미는 그 어휘적인(Lexical) 실행 문맥(Execution Context)에서의 의미가 된다. 그렇기 때문에 동일 범위(실행 문맥)의 모든 선언을 참고(호이스팅)해 의미를 정의한다.
- 실행 컨텍스트 생성 시 렉시컬 스코프 내의 선언이 끌어올려지는 게 호이스팅이다.
- 프로토타입 기반 언어인 자바스크립트에서는 '단어의 의미가 사용되는 근처 환경'에서 '근처'를 어휘적인 범위(Lexical Scope)로 정의했다.
- 자바스크립트 엔진은 코드가 로드될 때 실행 컨텍스트를 생성하고 그 안에 선언된 변수, 함수를 실행 컨텍스트의 최상단으로 호이스팅한다. 이러한 범위를 렉시컬 스코프라 한다.

#### 자바스크립트 — this

- 미리 분류하고 정의한 클래스를 가장 중요하게 여기는 전통적인 방식과는 달리, 프로토타입에서는 받아들이는 주체와 문맥이 가장 중요하다. 프로그래밍으로 보자면 실행(invoke)하는 '객체'가 중요하다.
- 프로토타입 기반 언어에서는 this가 정의된 함수가 어떻게 발화(invoke)되었는지에 따라 가리키는 값이 달라진다. 정확히는 받아들이는 대상의 컨텍스트를 가리킨다.