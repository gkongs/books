---
description: 타입 변환에 대해 알아봅시다.
---

# 타입 변환
자바스크립트의 모든 값은 타입이 있다. 값은 타입은 개발자의 의도에 따라 변환할 수도 있고, 문맥에 맞게 암묵접으로 변환될 수도 있다. <br>
의도적으로 변환하는 것을 **명시적 타입 변환 또는 타입 캐스팅** <br>
암묵적으로 변환되는 것을 **암묵적 타입 변환 또는 타입 강제 변환** 이라고 한다. <br>

타입 변환시에 중요한 것은 명시적으로 변환할 지 암묵적으로 변환할 지가 아닌 변환된 결과를 예측할 수 있어야 한다는 것이다. <br>
언뜻 보기에는 암묵적으로 변환하는 것이 실제 코드 상에 비춰지지 않으므로 좋지 않다고 생각이 되나, 자바스크립트의 문법을 잘 이해하고 있는 개발자에겐 가독성 측면에서 더 효율적일 때도 있기 때문이다.

# 암묵적 타입 변환
자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있다. <br>

# 문자열 타입으로 변환
`+` 연산자는 산술 연산자이면서 동시에 **문자열 연결 연산자**이기도 하다. <br>
만약 피연산자 중 문자열이 있을 경우 **문자열 연결 연산자**로 동작하며 이 연산자는 결과 값으로 문자열을 생성한다. <br>
따라서 이 때 피연산자는 코드의 문맥상 문자열 타입이어야 하기에 암묵적 타입 변환이 발생한다. <br>
이외에도 템플릿 리터럴에 표현식을 삽입하면 표현식의 값이 문자열 타입으로 암묵적 타입 변환된다. <br>

# 숫자 타입으로 변환
숫자 타입으로 암묵적 타입 변환이 되는 경우는 산술 연산자가 들어간 코드이거나 비교 연산자를 통해 값을 비교해야 할 때이다. <br>
```
'1' > 0 // -> true
```
비교 연산자의 역할은 `Boolean` 값을 만드는 것이다. 여기서 사용된 `>` 비교 연산자는 피연산자의 크기를 비교하므로 코드는 문맥상 모두 숫자 타입이어야 한다. <br>
```
+'' // 0
+'0' // 0
+'1' // 1
+'string' // NaN

+null // 0
+undefined // NaN

+{} // NaN
+[] // 0
+[1, 2] // NaN
+(function(){}) // NaN
```
일반적으로 값이 없으면 `0`, 있으면 `1`로 변환되지만, 문자열 타입의 경우 숫자 타입으로 변환시 평가 가능한 숫자 리터럴이라면 그대로 변환된다 <br>
```
+'10' // 10` 
```
또한 `객체와 빈 배열이 아닌 배열, undefined`는 변환되지 않아 `NaN`이 된다. <br>

# 불리언 타입으로 변환
`if 문`이나 `for 문`과 같은 제어문 또는 삼항 조건 연산자의 조건식은 `boolean` 값으로 평가 되어야 하는 표현식이다. <br>
따라서 불리언 타입으로 암묵적 타입 변환이 발생한다. <br>
이 때 자바스크립트는 참으로 평가되는 값을 **Truthy 값**, 거짓으로 평가되는 값을 **Falsy 값**으로 나눠 구분한다. <br>

# 명시적 타입 변환
명시적 타입 변환 방법은 표준 빌트인 생성자 함수(String, Number, Boolean)를 **new 연산자** 없이 사용하거나 표준 빌트인 메서드(toStirng(), ...)를 사용하거나 암묵적 타입 변환(+, ...)을 사용하는 방법이 있다. <br>

