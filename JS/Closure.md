중첩함수가 이미 생명주기를 마감한 외부함수의 지역 변수를 참조하는 경우 이 중첩함수를 closure라고 부른다.
```js
const x = 1;

function outer() {
 const x = 10;
 const inner = function () {
   console.log(x);
 }

 return inner;
}

const closure = outer();
closure(); // 10
```