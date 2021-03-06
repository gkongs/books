---
description: 값, 리터럴, 표현식, 문에 대한 개념을 정확히 알아봅시다.
---

# 값
**값은 식(표현식. expression)이 평가되어 생성된 결과**를 말한다. <br>
**평가**란 식을 해석해서 값을 생성하거나 참조하는 것을 의미한다.
```
// 변수에는 10 + 20이 평가되어 생성된 숫자 값 30이 할당된다.
var sum = 10 + 20; 
```

<br>

위 예제에서 `sum` 변수에 할당되는건 평가된 값인 `30` 이다. <br>
그러므로 `10 + 20`은 할당 이전에 평가되어 값을 생성해야 한다.

# 리터럴
**리터럴(literal)은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 표기하는 표기법을 말한다.** <br>
```3 // 그냥 3이 아니다. 숫자 리터럴이다.``` <br>
우리가 3을 코드에 기술하면 자바스크립트 엔진은 **런타임**에 이를 평가해 숫자 값 3을 생성한다. <br>

# 표현식
**표현식(expression)은 값으로 평가될 수 있는 문(statement)이다. 즉, 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다.** <br>
앞서 말했듯 리터럴은 런타임에 평가되어 값을 생성한다고 했다. 따라서 **리터럴도 표현식**이다. <br>
```
var score = 50 + 50; // 50 + 50 은 숫자 값 100으로 평가된다 = 표현식
score // 숫자 값 100으로 평가됨. 변수 이름(식별자) = 표현식
```

위 예제처럼 **값으로 평가될 수 있는 문은 모두 표현식**이다.

> 추가적으로 표현식과 평가된 값은 '동치 관계' 이므로 문법적으로 같은 자리에 위치할 수 있다. <br>
```
var num = 1;
num + 1; // '+' 산술연산자 좌항에 숫자 값 대신 동치관계인 숫자값 으로 평가되는 식별자가 와도 ok!
```

# 문
**문(statement)은 프로그램을 구성하는 기본 단위이자 최소 실행 단위다.** <br>
문은 여러 **토큰**으로 구성된다. **토큰**은 문법적으로 더 이상 나눌 수 없는 코드의 기본 요소를 의미한다. <br>
- 토큰의 예: 키워드, 식별자, 연산자, 리터럴, 세미콜론, 마침표 등의 특수 기호.

문을 **명령문**이라고도 부른다. 즉, 문은 컴퓨터에게 내리는 명령이다. <br>

> **세미콜론** <br>
문의 종료를 나타낸다. 자바스크립트 엔진은 **세미콜론**으로 문이 종료한 위치를 파악하고 순차적으로 하나씩 문을 실행한다. 따라서 문 끝엔 항상 **세미콜론**을 붙여야한다. <br>
예외도 있다. 0개 이상의 문을 중괄호로 묶은 코드 블록은 붙이지 않는다. (ex: if문) <br> 왜냐하면 이런 코드 블록은 언제나 문의 종료를 의미하는 **자체 종결성**을 갖기 떄문이다. <br>
<br>
그런데 사실 세미콜론을 붙이는 건 옵션이다. <br>
왜냐하면 자바스크립트 엔진은 소스코드를 해석할 때 문의 종료 위치를 예상해서 세미콜론을 붙이는 **세미콜론 자동 삽입기능(ASI)**를 암묵적으로 수행하기 때문이다. <br>
하지만 간혹 예상이 일치하지 않아 소스코드가 예상과는 다르게 동작할 수 있다. <br>
때문에 논쟁이 많지만 세미콜론을 붙이는 걸 권장하는 분위기이다.

(**code formatter**를 사용해서 세미콜론을 자동으로 붙이는 것을 추천한다 👍)

# 표현식인 문과 표현식이 아닌 문
말장난 같지만 표현식은 문의 일부일 수도 있고 그 자체로 문이 될 수도 있다.
```
var x; // 선언문은 값으로 평가되지 않는다 따라서 표현식이 아닌 문이다.
x = 1 + 2; // 표현식이면서 완전한 문이다. ...?
```
솔직히 위 예제를 보고 할당문이 표현식이란걸 잘 모르겠다. <br>
만일 자신도 잘 모르겠다면 변수에 할당해보면 된다. <br>
```
var foo = var x; // SyntaxError!
var foo = x = 1 + 2; // 3. 이로써 할당문은 할당한 값으로 평가되는 표현식이라는 걸 알 수 있다.
```

> **완료 값** <br>
크롬 개발자 도구에서 표현식이 아닌 문을 실행하면 `undefined`가 출력 된다. <br>
크롬 개발자 도구에서 표현식인 문을 실행하면 언제나 평가된 값을 반환한다.

