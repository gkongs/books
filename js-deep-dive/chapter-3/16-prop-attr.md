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

# 데이터 프로퍼티와 접근자 프로퍼티
프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.
- 데이터 프로퍼티: 키와 값으로 구성된 일반적인 프로퍼티다.
- 접근자 프로퍼티: 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

### 데이터 프로퍼티
데이터 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다. 이는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

> 프로퍼티 어트리뷰트 `[[Value]]` <br>
디스크립터 객체의 프로퍼티는 `value`이며 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다.<br>
프로퍼티 키를 통해 프로퍼티 값을 변경하면 `[[Value]]`에 값을 재할당한다. 이 때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 `[[Value]]`에 값을 저장한다.  <br>

> 프로퍼티 어트리뷰트 `[[Writable]]` <br>
디스크립터 객체의 프로퍼티는 `writable`이며 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다. <br>
`[[Writable]]`의 값이 false일 경우 해당 프로퍼티의 `[[Value]]`의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다. <br>

> 프로퍼티 어트리뷰트 `[[Enumerable]]` <br>
디스크립터 객체의 프로퍼티는 `enumerable`이며 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다. <br>
`[[Enumerable]]`의 값이 false인 경우 해당 프로퍼티는 `for ... in`문이나 `Object.keys` 메서드 등으로 열거할 수 없다. <br>

> 프로퍼티 어트리뷰트 `[[Configurable]]` <br>
디스크립터 객체의 프로퍼티는 `configurable`이며 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다. <br>
`[[Configurable]]`의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. `[[Writable]]`이 true인 경우 `[[Value]]`의 변경과 `[[Writable]]`을 false로 변경하는 것은 허용된다. <br>

# 접근자 프로퍼티

> 프로퍼티 어트리뷰트 `[[Get]]` <br>
디스크립터 객체의 프로퍼티는 `get`이며 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 `[[Get]]`의 값, 즉 `getter` 함수가 호출되고 결과가 반환된다. <br>

> 프로퍼티 어트리뷰트 `[[Set]]` <br>
디스크립터 객체의 프로퍼티는 `set`이며 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수다. 즉, 앞의 `getter`처럼 `[[Set]]`은 `setter`의 역할을 한다.

그리고 데이터 프로퍼티와 마찬가지로 `[[Enumerable]]`,`[[Configurable]]`이 있다. <br>

```
const person = {
    // 데이터 프로퍼티
    firstName: 'Ungmo',

    // 접근자 프로퍼티
    get fullName() {
        return ...
    },
    set fullName() {
        ...
    }
}
```

> **일반 객체의 __proto__는 접근자 프로퍼티고 함수 객체의 prototype은 데이터 프로퍼티**다. <br>

# 프로퍼티 정의
프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다. <br>
`Object.defineProperty` 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. <br>
(여러 개: `Object.defineProperties`) <br>

# 객체 변경 방지
객체는 유연하기 때문에 언제 어디서든 변경이 가능하다. 때문에 자바스크립트는 이를 방지하는 다양한 메서드를 제공한다. <br>

|구분|메서드|프로퍼티 추가|프로퍼티 삭제|프로퍼티 값 읽기|프로퍼티 값 쓰기|프로퍼티 어트리뷰트 재정의|
|---|---|-----------|---------|------------|------------|--------------------|
|객체 확장 금지|`Object.preventExtensions`|X|O|O|O|O|
|객체 밀봉|`Object.seal`|X|X|O|O|X|
|객체 동결|`Object.freeze`|X|X|O|X|X|

### 객체 확장 금지
`Object.preventExtensions`는 객체의 확장을 금지한다. 확장이 금지된 객체는 프로퍼티 추가가 금지된다. <br>
확장 가능한 객체인지를 알아보기 위해선 `Object.isExtensible` 메서드를 사용하면 된다. 
```
Object.isExtensible(objectName);
```

### 객체 밀봉
`Object.seal`을 통해 객체 밀봉을 하게되면 오직 **읽기와 쓰기만** 가능하다. <br>
`Object.isSealed`로 밀봉 여부를 알 수 있다. <br>

```
Object.isSealed(objectName);
```

### 객체 동결
`Object.freeze` 메서드는 객체 동결이다. **동결된 객체는 읽기만** 가능하다. <br>
`Object.isFrozen`로 동결 여부를 알 수 있다. <br>

```
Object.isFrozen(objectName);s
```

# 불변 객체
지금까지 알아본 방지 메서드들은 **얕은 방지** 방법이다. 따라서 중첩된 객체의 내부 객체들에 대해선 변경을 방지하지 못한다. <br>
때문에 중첩 객체까지 방지하는 즉, **깊은 방지**를 하고 싶다면 `Object.freeze`메서드를 모든 프로퍼티에 대해서 재귀적으로 호출해야한다. <br>

```
function deepFreeze(target) {
    if(target && typeof target === 'object' && !Object.isFrozen(target)) {
        Object.freeze(target);
        Object.keys(target).forEach(key => deepFreeze(target[key]));
    }
    return target;
}

const person = {
    name: 'Lee',
    address: { city: 'Seoul' }
};

// 깊은 객체 동결
deepFreeze(person);
```
