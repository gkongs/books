---
description: 데이터 타입에 대해 알아봅시다.
---

# 데이터 타입

**데이터 타입 (줄여서 타입)**은 **값의 종류**다. <br>
자바스크립트의 모든 값은 타입을 가진다. <br>
타입은 es11에 추가된 `Bigint`를 포함해 총 8개가 있다.

# 숫자 타입
자바스크립트는 `Number`라는 하나의 숫자 타입만 제공한다. <br>
`Number`는 수를 실수로 처리하기 때문에 정수를 위한 타입은 따로 없다. (int 같은) <br> 숫자 타입은 추가적으로 세 가지 특별한 값도 표현가능하다. <br>

- Infinity: 양의 무한대
- -Infinity: 음의 무한대
- NaN: 산술 연산 불가 (not-a-number)

# 문자열 타입
**문자열은 0개 이상의 16비트 유니코드 문자의 집합**이다. <br>
문자열은 키워드나 식별자 같은 토큰과 구분하기 위해 다른 타입과 다르게 쿼터 또는 백틱으로 감싸야 한다. (ex. "강아지")

# 불리언 타입
**참, 거짓**을 나타낸다.

# undefined 타입
초기화되면서 할당되는 값으로 가끔 직접 `undefined` 값을 할당하려고 하는 개발자가 있는데, 이건 `undefined`의 의미를 퇴색시키는 것이므로 어떤 값이 없다는 걸 표현하고 싶다면 `null` 타입을 사용하는게 좋다.

# null 타입
**값이 없다**라는 걸 의도적으로 표현하기 위한 타입이다.

# symbol 타입
다른 값과 중복되지 않는 유일한 값이다.

# 객체 타입
자바스크립트는 **원시 타입과 객체 타입**으로 분류한다. <br>
이에 대해서는 11장에서 다룬다고 한다..

# 데이터 타입의 필요성
근데 데이터 타입은 왜 필요할까? <br>
이유를 세 가지로 정리해보면 다음과 같다.

1. **값을 저장할 메모리 공간의 크기를 결정**
2. **값을 참조할 때 한 번에 읽어야 할 메모리 공간의 크기를 결정**
3. **메모리에서 읽은 2진수를 어떻게 해석할지 결정**

값은 각 메모리 공간에 2진수로 저장되는데 타입에 따라 해석이 달라지며 값도 달라진다. <br>
가령 `0100 0001`을 숫자로 해석하면 `65`, 문자열로 해석하면 `A`이다.  

# 동적 타입 언어
자바스크립트는 변수 선언시 타입을 정하지 않는 `동적 타입 언어`이다. <br>
이게 사용자 입장에선 편할지 모르지만 모든 소프트웨어의 아키텍처에는 `트레이드오프`가 존재하기에 이 또한 구조적 단점이 있다. <br>
동적 타입 언어는 타입이 언제든지 변경 될 수 있고 그만큼 **신뢰성**이 떨어지며 예상치 못한 오류가 발생할 가능성도 있다. <br>
따라서 동적 타입 언어는 항상 조심히 사용해야 한다. <br>

다음 내용은 주의 사항을 짧게 요약한 것이다.

- 변수는 제한적으로 사용해라.
- 변수를 제한적인 스코프를 갖도록 생성해라.
- 전역 변수는 최대한 사용하지 말라.
- 상수를 사용해라.
- 네이밍을 아기이름 짓듯이 하자.

---

<br>

**" 개념을 이해한다는 것은 바로 용어를 정확히 이해하고 설명할 수 있다는 것이다. "**

**" 컴퓨터가 이해하는 코드는 누구나 쓸 수 있다. 훌륭한 프로그래머는 사람이 이해할 수 있는 코드를 쓴다."**