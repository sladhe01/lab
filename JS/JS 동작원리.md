동작원리에 대해서 설명하기에 앞서 도식화된 동작 구조와 대략적인 개념을 알아보자.
<img style="display: block;-webkit-user-select: none;margin: auto;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;" src="https://raw.githubusercontent.com/sladhe01/lab/refs/heads/main/images/1_eFvdHDgxCM6C20Zt80I7Jg.webp">
기본적으로 자바스크립트는 싱글 스레드의 동기적 언어로 하나의 스택을 가지고 한번에 하나의 코드를 실행한다.
- Memory Heap : 메모리 할당이 일어나는 곳 (변수, 함수 등의 객체들이 담긴다)
  \* 자료구조중 하나인 Heap과는 별개의 용어
- Call Stack : 실행할 코드가 한줄 씩 쌓아서 실행하는 공간으로 함수의 실행이 끝나면 해당 함수는 스택에서 제거된다.
  \* 실제 쌓이는 것은 코드가 아니라 함수 실행정보가 담긴 **실행 컨텍스트가 쌓인다**.
- Web APIs : 비동기 처리를 담당한다. Node.js에서는 백그라운드가 담당한다.
  \* JS엔진이 직접 멀티 스레드를 쓰는 건 아니고 브라우저나 Node.js가 대신 멀티 스레드를 활용하는 것이다.
- Queue : 비동기 처리가 끝난 후 실행되어야 할 콜백 함수가 차례대로 할당된다. (바로 실행되지 않는 코드의 대기실 개념)
- Event Loop : Queue에 할당된 함수를 순서에 맞춰 Call Stack에 할당해준다.

###### 이제 예제를 통해 콜스택에서 무슨일이 일어나는지 부터 알아보자.

```js
function first() {
  second();
  console.log("first");
}

function second() {
  third();
  console.log("second");
}

function third() {
  console.log("third");
}

first();
```

1. JS가 실행되면 전역 실행 컨텍스트가 먼정 생성된다. 그리고 콜스택에는 전역 실행컨텍스트가 가장 아래애 쌓인다.
   브라우저 환경에서는 aonymous라는 명칭으로 쌓이는데 어떤 함수내부에서 실행되는 코드의 경우 감싸고 있는 함수가 콜스택의 가장 아래에 쌓이는데 전역 실행컨텍스트의 경우 어떤 함수 내부가 아니기 때문에 이름이 없다는 익명이란 뜻으로 anonymous라는 명칭으로 콜스택에 나타난다. Node.js환경에서는 모든 코드가 [[module wrapper function]] 안에서 실행되기 때문에 anonymous는 아니고 '실행파일명:0'와 같은 형태로 표시된다.
2. first(), second(), third(), console.log("third") 순서로 콜스택이 쌓인다. 각 함수의 호출을 대기중
3. "third"로그 출력 후 console.log()의 실행이 끝나면서 콜스택에서 제거 된다.
4. third() 함수의 실행이 끝나면서 콜스택에서 제거된다.
5. console.log("second")함수의 실행 컨텍스트가 콜스택에 추가되고 로그 출력 후 console.log()의 실행이 끝나면서 콜스택에서 제거된다.
6. second() 함수의 실행이 끝나면서 콜스택에서 제거된다.
7. console.log("first")함수의 실행 컨텍스트가 콜스택에 추가되고 로그 출력 후 console.log()의 실행이 끝나면서 콜스택에서 제거된다.
8. first() 함수의 실행이 끝나면서 콜스택에서 제거된다.
9. 마지막으로 전역 실행 컨텍스트(anonymous)가 콜스택에서 제거된다.
\* Uncaught RangeError: Maximum call stack size exceeded 에러는 JS엔진의 콜스택 한계를 넘겼을 경우 종료되면서 나타나는 에러로 연산이 오래걸리거나 반복문의 횟수가 많을 경우 발생할 수 있으므로 주의해야 한다.
###### 이번에는 다른 예제를 통해 비동기적인 함수를 실행할 경우 어떤 일이 일어나는지 알아보자.

```js
console.log("start");

setTimeout(function () {
  console.log("middle");
}, 3000);

console.log("end");
```
1. anonymous가 콜스택에 가장 먼저 쌓인다.
2. console.log("start")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
3. setTimeout() 함수가