Closure란 중첩 함수(inner)가 상위 스코프의 식별자를 참조하고 있고 외부 함수(outer)보다 더 오래 살아 있는 경우를 의미하는 개념이다.
아래 예시를 보자.
```js
const x = 1;

function outer() {
 const x = 10;
 const inner = function () {
   console.log(x);
 }

 return inner;
}
//---(1)
const quiz = outer(); // ---(2)
quiz(); // 10 ---(3)
```
(1)에서 실행 컨텍스트 스택에는 전역 실행 컨텍스트만 담겨있다.
(2)에서 outer( ) 실행 컨텍스트가 담기고, quiz에는 outer 함수의 반환 값인 inner 함수가 할당되며 inner 함수는 상위 스코프인 outer 함수의 지역 변수인 x의 값 10을 참조하여 10을 출력하는 함수가 된다. outer( )실행을 마치며 outer( ) 실행컨텍스트는 스택에서 사라지며 outer함수의 생명주기는 마감된다.
(3)의 시점에서 outer의 생명주기는 이미 마감되었지만 quiz 변수에 할당된 inner는 여전히 상위 스코프인 outer 함수의 지역변수 x를 참조하고 있다. 이것이 바로 closure다.