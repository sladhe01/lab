배열의 모든 요소마다 제공된 함수를 호출하는 메서드이다.

## Syntax

```js
forEach(callbackFn)
forEach(callbackFn, thisArg)
```

- callbackFn : 모든 요소마다 한번씩 호출하는 콜백함수다. 반환값을 갖지 않는다. 콜백함수는 아래와 같은 매개변수를 가진다.
  - element : 현재 처리중인 배열의 요소
  - index : 현재 처리중인 배열의 인덱스
  - array : forEach()를 호출한 배열

- thisArg : 콜백함수 실행 시 this로 사용되는 값

## Examples
