---
description: 생성자 함수에 의한 객체 생성의 장단점에 대해 알아봅시다.
---

# 생성자 함수
생성자 함수란 `new 연산자`와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다. <br>

### 객체 리터럴에 의한 객체 생성 방식의 문제점
객체 리터럴 생성 방식은 직관적이고 간단하다. 하지만 단 하나의 객체만 생성한다. 때문에 객체를 여러 개 생성해야 하는 경우 비효율적이다. <br>

### 생성자 함수 객체 생성 방식의 장점
생성자 함수로 객체를 생성하면 마치 템플릿 처럼 활용이 가능하다. <br>

```
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가르킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

const circle1 = new Circle(5);
const circle1 = new Circle(10); // 👍
```
자바스크립트는 클래스 기반 객체지향언어의 생성자와는 다르게 그 형식이 정해져 있는게 아니라서 만약 생성자 함수를 정의하고 호출 시 `new 연산자`를 붙이지 않는다면 일반 함수처럼 동작한다. <br>

```
// 위 예제와 이어집니다.
const circle3 = Circle(15);

// return문이 없는 일반 함수가 되었기 때문에
console.log(circle3); // undefined

// 일반 함수가 되기 때문에 Circle 내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

# 생성자 함수의 인스턴스 생성 과정
생성자 함수는 **인스턴스를 생성하는 것과 생성된 인스턴스를 초기화**하는 역할을 가진다. <br>
자바스크립트는 생성자 함수와 `new 연산자`를 함께 사용하면 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한 후 인스턴스를 반환한다. 그리고 그 순서는 아래와 같다.<br>

1. 인스턴스 생성과 this 바인딩  <br>
암묵적으로 빈 객체가 생성된다. 그리고 this에 바인딩된다. 이 빈 객체는 바로 생성자 함수가 생성한 인스턴스다. <br>
그래서 생성자 함수 내부의 this가 생성자 함수가 생성할 인스턴스를 가르키는 것이다. 그리고 이 동작은 런타임 이전에 진행된다. <br>

> 바인딩 <br>
바인딩은 식별자와 값을 연결하는 과정을 의미한다. this는 키워드로 분류되지만 식별자 역할을 하기에 this와 this가 가르킬 객체를 바인딩한다고 한다. 

2. 인스턴스 초기화 <br>
함수 내 기술되어 있는 코드를 한 줄씩 실행하며 초기화한다. <br>

3. 인스턴스 반환 <br>
생성자 함수 내부의 모든 처리가 끝나면 암묵적으로 인스턴스가 바인딩된 this를 반환한다. <br>

함수 내부에 this가 아닌 다른 객체를 return 하면 그 객체가 반환된다. 이는 자바스크립트의 기본 동작을 훼손하므로 생성자 함수 내부에서 return 문은 반드시 생략해야 한다. <br>