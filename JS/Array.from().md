iterable 혹은 배열과 유사한 객체를 배열의 형태로 만들어 반환하는 메서드다.

### Syntax

```js
Array.from(arrayLike, mapFunc, thisArg)
```
- arrayLike: iterable 혹은 배열유사 객체

- mapFunc(optional):  각 요소를 매핑하는 함수로  이 함수의 반환값이 Array.from()의 반환 배열의 요소로 대신 들어간다. 이 함수의 매개변수는 다음과 같다.
  - element: 배열에서 현재 처리중인 요소
  - index: 배열에서 현재 처리중인 요소의 index 

- thisArg(optional): mapFunc를 실행할 때 [[this]]로 사용되는 값.

### Examples
#### String
```js
Array.from('foo');
//["f", "o", "o"]
```

#### Set
```js
const set = new Set(["foo", "bar", "baz", "foo"]);
Array.from(set);
// [ "foo", "bar", "baz" ]
```

#### Map
```js
const map = new Map([
  [1, 2],
  [2, 4],
  [4, 8],
]);
Array.from(map);
// [[1, 2], [2, 4], [4, 8]]

const mapper = new Map([
  ["1", "a"],
  ["2", "b"],
]);
Array.from(mapper.values());
// ['a', 'b'];

Array.from(mapper.keys());
// ['1', '2'];
```

