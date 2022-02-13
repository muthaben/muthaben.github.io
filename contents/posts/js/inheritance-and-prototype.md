---
title: "[TIR] 상속과 프로토타입"
date: 2022-02-01
update: 2021-02-01
tags:
  - JavaScript
  - TIR
series: TIR
---

상속(inheritance)은 객체지향 프로그래밍의 핵심 개념으로 **어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것**을 말한다.  
자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다. 중복을 제거하는 방법은 **기존의 코드를 적극적으로 재사용**하는 것이다.

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return math.PI * this.radius ** 2
}

// 인스턴스 생성
const circle1 = new Circle(1)
const circle2 = new Circle(2)

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea) // true

console.log(circle1.getArea()) // 3.141592653...
console.log(circle2.getArea()) // 12.56637061...
```

Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.  
getArea 메서드는 단 하나만 생성되어 프로토타입인 Circle.prototype의 메서드로 할당되어 있다. 따라서 Circle 생성자 함수가 생성하는 모든 인스턴스는 getArea 메서드를 상속받아 사용할 수 있다. 즉, 자신의 상태를 나타내는 radius 프로퍼티만 개별적으로 소유하고 **내용이 동일한 메서드는 상속을 통해 공유**하여 사용하는 것이다.

> 프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공한다.  
> 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 여기에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.

- 객체 리터럴에 의해 생성된 객체의 프로토타입: Object.prototype
- 생성자 함수에 의해 생성된 객체의 프로토타입: 생성자 함수의 prototype 프로퍼티에 반영되어 있는 객체

**모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.**

<출처>

- 이웅모, 모던 자바스크립트 Deep Dive, 위키북스(2020)
