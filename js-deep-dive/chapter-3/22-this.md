---
description: this에 대해 알아봅시다.
---

# this
객체의 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다. <br>
객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다. <br>

`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로파티나 메서드를 참조할 수 있다. <br>

this는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다. 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다. 함수 내부에서 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼 this도 지역 변수처럼 사용할 수 있다. 단 this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다. <br>

> this 바인딩 <br>
**바인딩**이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this 바인딩은 this(키워드로 분류되지만 식별자 역할을 한다)와 가리킬 객체를 바인딩하는 것이다. <br>

객체 리터럴의 메서드 내부에서의 this는 메서드를 호출한 객체를 가르키며, 생성자 함수 내부에서 사용된 this는 생성할 인스턴스를 가리킨다.

# 함수 방식과 this 바인딩
**this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.**

> **렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.** <br>
함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 this 바인딩은 함수 호출 시점에 결정된다. <br>
(렉시컬 스코프는 함수 객체가 생성되는 시점, this 바인딩은 함수 호출 시점) <br>
주의할 점은 동일한 함수도 다양한 방식으로 호출할 수 있다는 것이다. 함수를 호출하는 방식은 다음과 같이 다양하다. <br>
> 1. 일반 함수 호출
> 2. 메서드 호출
> 3. 생성자 함수 호출
> 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```js
const foo = function () {
    console.dir(this);
}

// 1. 일반 함수 호출
foo(); // window - 전역 객체를 가르킨다. 단, strict mode에서는 undefined 이다. 

// 2. 메서드 호출
const obj = { foo }; 
obj.foo(); // obj - 메서드의 객체를 가르킨다.

// 3. 생성자 함수 호출
new foo(); // foo {} - 생성자 함수가 생성한 인스턴스를 가르킨다.

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
const bar = { name: 'bar' };

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar); // bar
```

### 일반 함수 호출
this를 일반 함수에서 호출하면 전역 객체를 가르킨다. 이건 객체의 메서드 내에서 정의한 중첩 함수나 콜백 함수의 경우에도 마찬가지이다. 일단 일반 함수로 호출이 되면 this는 전역객체를 가르킨다. <br>
```js
const obj = {
    foo() {
        console.log(this): // obj

        function bar() {
            console.log(this); // window!
        }

        setTimeout(function () {
            console.log(this); // window!
        }, 1000);
    }
};
```
위와 같이 일반 함수로 호출된 모든 함수 내부의 this에는 전역 객체가 바인딩된다. 이 동작은 대부분 헬퍼함수의 역할을 하는 콜백, 중첩함수의 동작을 제대로 할 수 없도록 만들기도 한다. 때문에 `Function.prototype.call, Function.prototype.apply, Function.prototype.bind` 메서드를 통해 this를 명시적으로 바인딩하거나 메서드 내부에서 this를 변수에 담아서 사용하거나 화살표 함수를 사용하여 this를 바인딩한다. <br>

```js
const obj = {
    foo() {
        const that = this;
        
        setTimeout(function () {
            console.log(that); // obj
        }, 1000);

        setTimeout(function () {
            console.log(this); // obj
        }.bind(this), 1000);
    
       setTimeout(() => console.log(this) /* obj */, 1000);
    }
};
```

### 메서드 호출
메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다. <br>
메서드는 프로퍼티에 바인딩된 함수. 즉, 독립적으로 존재하는 별도의 객체다. 메서드는 프로퍼티가 함수 객체를 가르키고 있을뿐인 것이다. <br>

### 생성자 함수 호출
생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩 된다.

### Function.prototype.apply/call/bind 메서드에 의한 간접 호출
`apply`, `call`의 본질적인 기능은 함수를 호출하는 것이다. 단 첫 번째 인수로 전달한 특정 객체를 this에 바인딩 시킨다.
```js
function getThisBinding() {
    return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding()); // window
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```
둘의 차이점은 두 번째 인수로 함수의 인수를 전달할 수 있는데, `apply`는 [] 형태로, `call`은 comma로 나눈 리스트 형태로 전달하는 것이다.
```js
console.log(getThisBinding.apply(thisArg, [1, 2, 3])); // [] array 형태
console.log(getThisBinding.apply(thisArg, 1, 2, 3));  // , list 형태
```
`bind`는 함수를 호출하지 않는다. 다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다. <br>
```js
console.log(getThisBinding.bind(thisArg)); // getThisBinding
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```
`bind` 메서드는 앞서 나왔던 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this 불일치 문제를 해결하기 위해 유용하게 사용된다. <br>
