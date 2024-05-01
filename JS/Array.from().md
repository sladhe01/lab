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

#### [[Set]]
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

#### [[arguments]]
```js
function f() {
  return Array.from(arguments);
}

f(1, 2, 3);

// [ 1, 2, 3 ]
```

#### mapFunc(optional)
```
Array.from([1, 2, 3], (x) => x + x);
// [2, 4, 6]

Array.from({ length: 5 }, (v, i) => i);
// [0, 1, 2, 3, 4]
/* 
Array.from은 첫번째 인자(arrayLike)의 .length property를 가져오게 
되어있는데 length라는 속성이 5라는 값을 가지도록 만든 fake array를 사용하여
배열의 길이가 5인 임의의 유사 배열 객체를 이용했고
mapFunc의 첫번째 매개변수는 요소, 두번째 매개변수는 인덱스로 이 예시의 mpaFunc은
배열의 인덱스를 반환하도록 하고있다 
*/
```