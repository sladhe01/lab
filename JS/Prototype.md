<br/>

### prototype object
프로토타입 기반 객체지향 언어인 JS는 클래스 없이도 객체를 생성할 수 있다.(Java와 같은 클래스 기반 객체지향 언어는 클래스가 있어야 한다.)
JS의 모든 객체는 부모 역할을 하는 객체와 연결되어 있고 부모 객체의 프로퍼티, 메서드를 상속받아 사용가능한데 이러한 부모 역할을 하는 객체를 Prototype 객체라고 부른다.
다음은 ECMAScript의 공식문서에 나와있는 내용이다.
- JS의 모든 객체는`[[ProtoType]]`이라는 internal slot을 가진다.
- `[[ProtoType]]`의 값은 null 또는 객체이며 상속을 구현하는데 사용된다.
- `[[ProtoType]]`객체의 데이터 프로퍼티는 get 액세스를 위해 상속되어 자식 객체의 프로퍼티처럼 사용될 수 있다. 하지만 set 액세스는 허용되지 않는다.
`[[Prototype]]`이 null이 아니라면 프로토타입 객체이고 `__proto__` 속성을 이용하여 접근할 수 있다.
`__proto__` 속성으으로 접근하면 내부적으로 **Object.getPrototypeOf** 가 호출되어 프로토타입 객체를 반환한다.
```
let person = {
	name: 'Jeong',프로토타입
	age: 31
}
console.log(person.__proto__ === Object.prototype);//true
```
객체를 생성할 떄 프로토 타입은 정해지고 프로토타입 객체는 다른 임의의 객체로 변경이 가능하다. 즉 부모 개체인 프로토타입 객체를 동적으로 변경할 수 있다는 뜻이다.
<br/>

### prototype property
상기 서술한대로 모든 객체는 프로토타입 객체를 지칭하는 `[[prototype]]` internal slot을 가지고 있으며 `__proto__`속성을 통해 접근이 가능하다고 했다.
하지만 함수 객체는 일반 객체와 달리 prototype 속성도 갖게 된다.
여기서 오해하면 안되는 것은 prototype속성을 지칭하는`[[prototype]]` internal slot은 함수의 prototype 속성과 다르다는 점이다.
```js
function Person(name) {
	this.name = name;
}

var foo = new Person('Jeong')

console.log(Person)
console.log(foo)
```
	
- `[[prototype]]`
	- 모든 객체가 가지고 있는 internal slot
	- 객체의 입장에서 자신의 부모 역할을 하는 prototype object를 지칭하며 함수의 경우 Function.prototype을 지칭한다
```js
console.log(Person.__proto__ === Function.prototype); //true
```
- prototype property
	- 함수 객체만 가지고 있는 속성이다.
	- 함수 객체가 생성자로 사용될 때 이 함수를 통해 생성될 객체의 부모 역할을 하는 객체(prototype 객체)를 지칭한다.
```js
console.log(Person.prototype === foo.__proto__); //true
```

### constructor property

prototype 객체는 constructor라는 속성을 갖는데 객체를 생성한 객체를 지칭한다.
```js
fucntion Person(name) {
	this.name = name;
}

let foo = new Person("Jeong");

console.log(Person.prototype.constructor === Person); //true
/*
위에서 언급한 것과 같이 Person.prototype이 Person 함수를 통해 생성될 객체의 
부모역할을 하는 프로토타입 객체를 지칭하고 생성될 객체의 프로토타입 객체를 생성한 
객체는 Person 함수이기 떄문이다.
*/

console.log(foo.constructor)
```