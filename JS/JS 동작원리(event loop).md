JS는 싱글 스레드의 동기적 언어로 하나의 스택을 가지고 한번에 하나의 코드를 실행한다. 자세한 동작원리에 대해서 설명하기에 앞서 도식화된 동작 구조와 대략적인 개념을 알아보자.
<img style="display: block;-webkit-user-select: none;margin: auto;background-color: hsl(0, 0%, 90%);transition: background-color 300ms;" src="https://raw.githubusercontent.com/sladhe01/lab/refs/heads/main/images/1_eFvdHDgxCM6C20Zt80I7Jg.webp">
- Memory Heap : 메모리 할당이 일어나는 곳 (변수, 함수 등의 객체들이 담긴다)
  \* 자료구조중 하나인 Heap과는 별개의 용어
- Call Stack : 실행할 코드가 한줄 씩 쌓아서 실행하는 공간으로 함수의 실행이 끝나면 해당 함수는 스택에서 제거된다.
  \* 실제 쌓이는 것은 코드가 아니라 함수 실행정보가 담긴 **실행 컨텍스트가 쌓인다**.
- Web APIs : 비동기 처리를 담당한다. Node.js에서는 백그라운드가 담당한다.
  \* JS엔진이 직접 멀티 스레드를 쓰는 건 아니고 브라우저나 Node.js가 대신 멀티 스레드를 활용하는 것이다.
- Callback Queue : 비동기 처리가 끝난 후 실행되어야 할 콜백 함수가 차례대로 할당된다. (바로 실행되지 않는 코드의 대기실 개념. Microtask Queue, Animation Frame, Task Queue 등 처리하는 작업에 따라 구분되며 서술된 순서대로 이벤트루프가 우선 콜스택으로 할당해준다.)
- Event Loop : Queue에 할당된 함수를 순서에 맞춰 Call Stack에 할당해준다.
##### Inside Engine
JS 엔진 내부에서 소스코드는 [[Evaluate vs Execution |평가와 실행]]의 과정으로 나누어 처리된다.
평가 과정에서는 [[Execution Context |실행컨텍스트]]를 생성하고, 그 실행 컨텍스트에 있는 렉시컬 환경의 환경 레코드에 변수와 함수 식별자를 등록한다. 이 평가 과정이 끝나면 순차적으로 소스코드가 실행된다. 이것을 런타임이라고 하는데 이 과정에서 필요한 변수나 함수의 참조는 실행컨텍스트가 관리하는 스코프 체인에서 검색하여 취득하고 값의 변경이 발생하면 해당 스코프 체인 자료구조의 내용을 변경한다. 

##### Call Stack & Event Loop
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
9. 마지막으로 전역 실행 컨텍스트(anonymous)가 실행완료되며 콜스택에서 제거된다.
\* Uncaught RangeError: Maximum call stack size exceeded 에러는 JS엔진의 콜스택 한계를 넘겼을 경우 종료되면서 나타나는 에러로 연산이 오래걸리거나 반복문의 횟수가 많을 경우 발생할 수 있으므로 주의해야 한다.
###### 이번에는 다른 예제를 통해 비동기적인 함수를 실행할 경우 어떤 일이 일어나는지 알아보자.

```js
console.log("start");

setTimeout(function () {
  console.log("middle");
}, 3000);

console.log("end");
```

10. anonymous가 콜스택에 가장 먼저 쌓인다.
11. console.log("start")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
12. setTimeout() 함수는 비동기 함수로 콜스택에 쌓이지 않고 바로 WebAPI로 이동한다.
13. console.log("end")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
14. anonymous가 실행완료되며 콜스택에서 제거된다.
15. 3초가 지난 후WebAPI에 있던 setTImeout 내부의 콜백함수인 익명함수가 콜백큐로 이동한다.
16. 이벤트 루프가 콜스택이 비어있는지 확인 후 콜백큐에 있던 익명함수를 콜스택으로 넘겨 콜스택에는 익명함수가 쌓인다. (이벤트 루프는 여기서만 콜스택이 비어있는지 확인하는 것이 아니라 이전부터 계속 확인하고 있다.)
17. 익명함수 내부의 console.log("middle")이 콜스택에 쌓인다.
18. 콘솔 출력후 console.log() 콜스택에서 제거된다.
19. 익명함수가 종료되며 콜스택에서 제거된다.

###### 만약 setTimeout의 대기 시간이  3000이 아니라 0이 였다면 어떤 일이 벌어졌는지 알아보자

엔진과 브라우저 환경에 따라서 조금씩 다르겠지만 console.log("end")가 실행되기전에 Web API의 익명함수가 콜백큐로 먼저 넘어간다는 가정하에 아래 순서를 적어보겠다.

20. anonymous가 콜스택에 가장 먼저 쌓인다.
21. console.log("start")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
22. setTimeout() 함수는 비동기 함수로 콜스택에 쌓이지 않고 바로 WebAPI로 이동한다.
23. Web API에 있던 setTImeout() 내부의 콜백함수인 익명하수가 콜백큐로 이동한다.
24. console.log("end")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
25. anonymous가 실행완료되며 콜스택에서 제거된다.
26. 이벤트 루프가 콜스택이 비어있는지 확인 후 콜백큐에 있던 익명함수를 콜스택으로 넘겨 콜스택에는 익명함수가 쌓인다.
27. 익명함수 내부의 console.log("middle")이 콜스택에 쌓인다.
28. 콘솔 출력후 console.log() 콜스택에서 제거된다.
29. 익명함수가 종료되며 콜스택에서 제거된다.

###### 이번에는 또 다른 예제를 통해서 알아보자.

```js
console.log("start");

setTimeout(fuction () {
  console.log("middle");
}, 0);

Promise.resolve()
  .then(function () {
    console.log("promise");
  });

console.log("end");
```

30. anonymous가 콜스택에 가장 먼저 쌓인다.
31. console.log("start")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
32. setTimeout() 함수는 비동기 함수로 콜스택에 쌓이지 않고 바로 WebAPI로 이동한다.
33. Promise.resolve()가 콜스택에 쌓이고 즉시 Promise객체를 반환 하며 콜스택에서 제거된다.
34. .then()에 있는 콜백함수인 익명함수가 마이크로태스크 큐로 이동한다.
35. console.log("end")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
36. 익명함수가 종료되며 anonymous가 콜스택에서 제거된다.
37. 이벤트 루프가 콜스택이 빈것을 확인한 후 마이크로 태스크 큐의 .then()의 익명함수를 콜스택으로 이동시켜 콜스택에 쌓인다.
38. console.log("promise")가 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.
39. .then()의 익명함수가 콜스택에서 제거된다.
40. 이벤트루프가 콜스택이 빈것을 확인한 후 setTimeout의 익명함수를 콜스택으로 이동시켜 콜스택에 쌓인다.
41. console.log("middle")이 콜스택에 쌓이고 로그 출력 후 콜스택에서 제거된다.