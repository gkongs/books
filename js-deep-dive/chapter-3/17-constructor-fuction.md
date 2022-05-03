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

### 내부 메서드 `[[Call]]`과 `[[Construct]]`
함수 선언문 또는 함수 표현식으로 정의한 함수는 일반함수는 물론이고 생성자 함수로서도 호출 할 수 있다. <br>
함수는 객체지만 일반 객체와는 다르다. 일반 객체는 호출할 수 없지만 함수는 호출할 수 있기 때문이다. 따라서 함수는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론이고 함수 객체만을 위한 `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]`, `[[Construct]]`같은 내부 메서드를 추가로 가지고 있다. <br>
일반 함수로 호출될 땐 `[[Call]]` 메서드가 호출되고 생성자 함수로서 호출될 땐 `[[Construct]]`가 호출된다. 내부 메서드 `[[Call]]`을 갖는 함수 객체를 `Callable`이라 하며 내부 메서드 `[[Construct]]`를 갖는 함수 객체를 `constructor`, `[[Construct]]`를 갖지 않는 함수 객체를 `non-constructor`라고 한다. <br>
`non-constructor`는 객체를 생성자 함수로서 호출할 수 없는 함수를 의미한다. <br>
호출할 수 없는 함수 객체는 함수가 아니므로 모든 함수 객체는 `Callable`이다. 즉, 모두 `[[Call]]` 메서드를 가지고 있다. 반면에 `[[Construct]]`는 있을수도, 없을수도 있다. <br>

### constructor와 non-constructor의 구분
자바스크립트 엔진은 함수 `constructor` 와 `non-constructor`를 어떻게 나눌까? <br>
자바스크립트 엔진은 함수 정의를 평가하여 구분하는데 `constructor`는 **함수 선언문, 함수 표현식, 클래스(클래스도 함수다)**이고 `non-constructor`는 **메서드(ES6 메서드 축약표현), 화살표 함수**다. <br>
여기서 조심할 점은 ESMAscript 사양에서의 메서드는 일반적인 메서드보다 범위가 좁다는 것이다.

```
function foo() {} // 함수 선언문
const bar = fuction () {}; // 함수 표현식

const baz = {
    x: function () {} // 프로퍼티 x의 값으로 정의된 함수는 메서드가 아니라 일반 함수로 취급한다.
}

new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}

const arrow = () => {};

const obj = {
    x() {} // 축약 표현만 메서드로 인정한다.
};

new arrow(); // TypeError: arrow is not a constructor
new obj.x(); // TypeError: obj.x is not a constructor
```

### new 연산자
일반 함수와 생성자 함수에 특별한 형식적 차이가 없지만 `new 연산자`와 함께 함수를 호출하면 `[[Call]]` 대신 `[[Construct]]`가 호출된다. <br>

### new.target
생성자 함수는 파스칼 표기법으로 일반 함수와는 다르게 보이도록 구분하지만 그래도 `new 연산자` 없이 사용할 수 있다. 때문에 이를 막기 위한 방법으로 나온 것이 `new.target`이다. <br>
ES6부터 등장하였으며, IE에선 동작하지 않는다. 
```
function Circle(radius) {
    // new 연산자와 호출되지 않으면 new.target이 undefined기 때문에 분기를 통해 새로 호출한다.
    if (!new.target) {
        return new Circle(radius);
    }
}
```
참고로 대부분 빌트인 생성자 함수는 `new 연산자`와 함께 호출되었는지를 확인한 후 반환한다. <br>
하지만 `String, Number, Boolean`은 `new 연산자` 사용시 객체를 반환하지만, 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다. 때문에 이를 이용해서 타입 변환을 하기도 한다. <br>