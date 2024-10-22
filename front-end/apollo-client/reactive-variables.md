apollo client cache 밖에서도 local state를 나타낼 수 있는 유용한 메커니즘이다.
cache와 분리되어 있고 어떠한 타입과 구조의 데이터를 저장할 수 있다. 그리고 application의 어디에서나 gql 문법없이 상호작용가능하다.

필드의 값이 reactive variables에 의존하고 있다면 reactive variables의 값이 변경되면 그 필드를 갖는 쿼리들이 자동으로 새로고침된다.

```js
// cartItemsVar의 초기값을 []로 생성
const cartItemsVar = makeVar([]);

// 매개변수 없이 호출로 현재값 읽기
// Output: []
console.log(cartItemsVar());


// 매개변수를 더하여 호출로 값 수정
const cartItemIds = [100, 101, 102];
cartItemsVar(cartItemIds);

// Output: [100, 101, 102]
console.log(cartItemsVar());

```