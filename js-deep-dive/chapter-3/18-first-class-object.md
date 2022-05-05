---
description: 일급 객체에 대해 알아봅시다.
---

# 일급 객체
1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다. <br>

위 조건을 모두 만족하는 객체를 **일급 객체**라고 하는데 자바스크립트 에서는 함수가 다음 조건을 모두 만족하므로 **함수는 일급 객체**다. <br>

일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로 사용할 수도 있다는 것이다. <br>

# 함수 객체의 프로퍼티

### arguments 프로퍼티
arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다. 즉, 함수 외부에서는 참조할 수 없다. <br>
현재 함수 객체의 arguments 프로퍼티는 표준에서 폐지 되었기에 사용하려면 `Function.arguments`와 같은 방법이 아닌 함수 내부에서 지역 변수처럼 사용할 수 있는 `arguments` 객체를 참조하도록 한다. <br>

arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하다. <br>

### length 프로퍼티
함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다. <br>
arguments 객체의 length는 인자의 개수고 함수 객체의 length 프로퍼티는 매개변수의 개수다. <br>

### name 프로퍼티
함수 객체의 name 프로퍼티는 함수 이름을 나타낸다. ES6부터 표준이 되어서 ES5와 ES6부터의 동작이 다르다. <br>

### __proto__ 접근자 프로퍼티
모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 가진다. `[[Prototype]]` 내부 슬롯은 객체 지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다. <br>

`__proto__` 프로퍼티는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다. <br>

> `hasOwnProperty` 메서드 <br>
이름처럼 인수로 전달받은 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속 받은 프로퍼티 키면 false를 반환한다. <br>

### prototype 프로퍼티
prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만이 소유하는 프로퍼티이며 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다. <br>

