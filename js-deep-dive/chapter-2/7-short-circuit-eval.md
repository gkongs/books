---
description: 단축 평가에 대해 알아봅시다.
---

# 단축 평가
논리 연산자 `&& ||`의 결과 값은 불리언 값이 아닐 수도 있다. <br>
```
'Cat' && 'Dog' // 'Dog'. 결과 확정 시점이 우측 피연산자
'Cat' || 'Dog' // 'Cat'. 결과 확정 시점이 좌측 피연산자
```
위 예제처럼 논리곱(&&) 연산자와 논리합(||)연산자는 **논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 반환**하는데 이를 **단축 평가**라고 한다. <br>
이처럼 **단축 평가**는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다. <br>

### 단축 평가의 규칙
|단축 평가의 표현식| 평가 결과 |
|---|---|
|true \|\| anything | true |
|false \|\| anything | anything |
|true && anything | anything |
|false && anything |false |
<br>

# 단축 평가의 활용
단축 평가를 다음과 같은 상황에서 유용하게 사용할 수 있다.<br>
```
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null
```
위 예제에선 `elem`이라는 변수의 프로퍼티를 참조하려고 한다. 하지만 프로퍼티를 참조할 때 참조하려는 변수의 값이 `unll 또는 undefined`라면 `TypeError`가 발생한다. <br>
이 때 단축평가를 사용하면 에러를 발생시키지 않는다. <br>
```
var elem = null;
var value = elem && elem.value; // null
```

비슷한 경우로 함수의 기본값을 설정할 때도 사용할 수 있다.
```
function getStringLength(str) {
    str = str || '';
    return str.length;
}
```

> 디폴트 매개변수 <br>
ES6에서 추가된 디폴트 매개변수를 사용하면 위의 예제에서 나온 함수의 기본 값을 좀 더 보기 좋게 설정할 수 있다. <br>
>```
>function getStringLength(str = '') {
>   return str.length;
>}
>```
>보이는대로 `||`를 사용하는 것 보다 훨씬 간결하고 알아보기 쉽다. <br>
<br>
그렇다면 항상 디폴트 매개변수를 사용하는 것이 좋을까? <br>
그건 아니다. 왜냐하면 디폴트 매개변수는 값이 `undefined` 일 때만 동작하고, `falsy` 값에는 동작하지 않기 때문이다. <br>
따라서 상황에 맞게 사용해야한다. <br>

# 옵셔널 체이닝 연산자
ES11에서 도입된 **옵셔널 체이닝 연산자** `?.`는 앞서 나온 변수의 프로퍼티를 참조할 때 사용할 수 았는 유용한 연산자이다. <br>
```
var elem = null;
var value = elem?.value; // null
```
`?.`는 좌항의 피연산자가 `null 또는 undefined`인 경우 `undefined`를 반환하고 아니면 우항의 프로퍼티 참조를 이어간다. <br>
이처럼 `&&` 식보다 간단하게 표현할 수 있고 또한 `&&` 식은 `0, ''`를 falsy 값으로 처리하여 프로퍼티 참조를 하지않고 좌항을 반환하는데 사실 `0, ''`은 객체로 평가될 때도 있다.(이유는 뒤 챕터에 등장한다.) <br>
그래서 옵셔널 체이닝은 좌항이 `falsy` 값이라 할지라도 null 또는 undefined 값이 아니라면 우항의 프로퍼티 참조를 이어가도록 설계되어 있다. <br>

# null 병합 연산자
ES11에서 도입된 **null 병합 연산자** `??`는 변수의 기본 값을 설정하는데 유용하다. <br>
그러므로 함수의 디폴트 매개변수의 값을 정하는데도 유용한데 기존에 사용하던 `||`식 보다 코드적으로 더 간결해지진 않지만 옵셔널 체이닝과 마찬가지로 `falsy` 값이라 할지라도 null 또는 undefined 값이 아니라면 우항을 반환하지 않는다. <br>
기본 값은 `0 또는 ''`와 같이 `falsy` 값 중에서도 유효한 값이 있기 때문에 모든 `falsy` 값을 체크하는 `|| 단축 평가식`은 예기치 않은 동작이 발생할 수도 있다. <br>
따라서 이러한 점을 고려하여 보완되어 나온 **null 병합 연산자**를 사용하는 것이 좋다. 


