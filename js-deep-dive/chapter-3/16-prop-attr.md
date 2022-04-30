---
destription: 프로퍼티와 어트리뷰트에 대하여 알아봅시다.
---

# 내부 슬롯과 내부 메서드
프로퍼티와 어트리뷰트에 대해 이해하기 위해 먼저 **내부 슬롯(internal slot)과 내부 메서드(internal method)의 개념**에 대해 알아봅시다. <br>

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 **의사 프로퍼티와 의사 메서드**다. ECMAScript 사양에 등장하는 이중 대괄호 `([[...]])`로 감싼 이름들이 내부 슬롯과 내부 메서드다. <br>

이 내부 슬롯과 메서드는 외부로 공개된 객체의 멤버들이 아니다. 그래서 원칙적으로 자바스크립는 내부 슬롯과 메서드에 직접 접근하거나 호출할 수 있는 방법을 제공하지 않는다. 단, 일부 내부 슬롯과 메서드에 한하여 간접적으로 접근할 수 있는 방법을 제공하기도 한다. <br>
```
const o = {};

// 직접 접근 불가
o.[[Prototype]] // Uncaught SyntaxError: Unexpected token '['

// 간접적 접근
o.__proto__ // Object.prototype
```

# 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다. <br>

여기서 프로퍼티의 상태란 **프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)를 말한다.** <br>

프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값(meta-property)인 내부 슬롯 `[[value]]`,`[[Writable]]`,`[[Enumerable]]`,`[[configurable]]`이다. 따라서 프로퍼티 어트리뷰트를 직접 접근할 순 없지만 `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인할 수는 있다. <br>
```
const person = {
    name: 'Lee'
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```
`Object.getOwnPropertyDescriptor`의 첫 번째 인자는 객체의 참조를, 두 번째 매개변수에는 프로퍼티 키를 문자열로 전달한다. 이 때 위 메서드는 프로퍼티 어트리뷰트 정보를 제공하는 **프로퍼티 디스크립터 객체**를 반환한다. <br>
만약 존재하지 않는 프로퍼티나 상속받은 프로퍼티에 대한 프로퍼티 디스크립터를 요구하면 `undefined`를 반환된다. <br>

`Object.getOwnPropertyDescriptor`는 하나의 프로퍼티에 대해 프로퍼티 디스크립터를 반환하지만 ES8부터 추가된 `Object.getOwnPropertyDescriptors` 메서드는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다. <br>

```
const person = {
    name: 'Lee'
};

person.age = 20;

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
    name: {value: "Lee", writable: true, enumerable: true, configurable: true},
    age: {...}
}
*/
```


