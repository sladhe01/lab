arguments는 함수 내부에서 사용할 수 있는 전역 객체 속성으로 실행중인 함수에 전달되는 모든 인자(argument)를  요소로 담은 유사 배열 객체이다.
```js
function f() {
  return Array.from(arguments);
}

console.log(f(1, 2, 3)); // 출력: [1, 2, 3]
```