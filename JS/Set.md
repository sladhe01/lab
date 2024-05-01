set 객체는 [[Primative type|원시값]]이나 객체 참조  값 등 모든 유형의 중복되지 않는 고유 값을 저장할 때 사용된다.

```js
const mySet1 = new Set();

mySet1.add(1); // Set(1) { 1 }
mySet1.add(5); // Set(2) { 1, 5 }
mySet1.add(5); // Set(2) { 1, 5 }
mySet1.add("some text"); // Set(3) { 1, 5, 'some text' }
const o = { a: 1, b: 2 };
mySet1.add(o);

mySet1.add({ a: 1, b: 2 }); // 같은 키와 값을 가진 객체이지만 저장된 메모리 위치가 다르기 때문에 다른 o와 다른 객체를 참조한 것.

mySet1.has(1); // true
mySet1.has(3); // false, 3 이 set에 추가되지 않았기 때문
mySet1.has(5); // true
mySet1.has(Math.sqrt(25)); // true
mySet1.has("Some Text".toLowerCase()); // true
mySet1.has(o); // true

mySet1.size; // 5

mySet1.delete(5); // set에서 5 제거
mySet1.has(5); // false

mySet1.size; // 4

mySet1.add(5); // Set(5) { 1, 'some text', {...}, {...}, 5 } - 이전에 삭제된 아이템이 새로운 아이템으로 추가되지만 삭제 전 원래 위치를 유지하지는 못한다.

console.log(mySet1); // Set(5) { 1, "some text", {…}, {…}, 5 }
```