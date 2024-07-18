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
```js
let person = {
	name: 'Jeong',프로토타입
	age: 31
}
console.log(person.__proto__ === Object.prototype); //true
```
객체를 생성할 떄 프로토 타입은 정해지고 프로토타입 객체는 다른 임의의 객체로 변경이 가능하다. 즉 부모 개체인 프로토타입 객체를 동적으로 변경할 수 있다는 뜻이다.
<br/>

### prototype property
상기 서술한대로 모든 객체는 프로토타입 객체를 지칭하는 `[[Prototype]]` internal slot을 가지고 있으며 `__proto__`속성을 통해 접근이 가능하다고 했다.
하지만 함수 객체는 일반 객체와 달리 prototype 속성도 갖게 된다. (함수에만 존재하는 속성이다.)
여기서 오해하면 안되는 것은 prototype속성을 지칭하는`[[Prototype]]` internal slot은 함수의 prototype 속성과 다르다는 점이다.
```js
function Person(name) {
	this.name = name;
}

var foo = new Person('Jeong')

console.log(Person) // 함수 -> prototype 프로퍼티가 있다.
console.log(foo) // 인스턴스 -> prototype 프로퍼티가 없다.
```
	
- `[[Prototype]]`
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

prototype 객체는 constructor라는 속성을 갖는데 생성된 객체 입장에서 자신을 생성한 객체를 지칭한다.
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

console.log(foo.constructor === Person); //true
//foo를 생성한 객체는 Person() 생성자 함수이기 떄문이다.

console.log(Person.constructor === Funtion); //trune
//Person이라는 생성자 함수를 생성한 것은 Function() 생성자 함수이기 떄문이다.
```

### Prototype Chain

자바스크립트는 특정 객체의 프로퍼티나 메서드에 접글할 때 해당 객체에 그 프로퍼티나 메서드가 없다면 `[[Prototype]]`이 가리키는 링크를 따라 자신의 부모역할을 하는 프로토타입 객체의 프로퍼티나 메서드를 차례대로 검색하는데 이것을 프로토타입 체인이라고 한다.
```js
let student = {
	name: "Jeong",
	score: 93
}

console.log(student.hasOwnProperty('name')); //true
/*
[[Prototype]] internal slot이 가리키는 링크를 따라가서 student 객체의 부모역할을 하는
프로토타입 객체인 Object.prototype의 메서드인 hasOwnProperty를 호출한 것이다.
*/
console.log(student.__proto__ === Object.prototype); //true
console.log(Object.prototype.hasOwnProperty('hasOwnproperty')); //ture
```


JS는 객체를 생성하는 세가지 방법이 있다.
 - 객체 리터럴을 이용하여 생성하는 방법
 - 생성자 함수를 통하여 생성하는 방법
 - Object( ) 생성자 함수를 이용하여 생성하는 방법
여기서 객체 리터럴을 생성 하는 방법은  내장함수를 이용하여 Obejct( )생성자 함수를 이용하여 생성한다.

객체가 생성되는 방법에 따라서 prototype chain이 일어나는 방식이 조금 다르다.
- 객체 리터럴을 이용하여 생성할 때(Object( )생성자를 이용하여 생성할 때)
	- 위 방식을 통하여 생성된 객체의 프로토타입 객체는 Object.prototype이다.
```js
let person = {
	name: 'Jeong',
	gender: 'male',
	sayHello: function(){
		console.log('Hi my name is' + this.name);
	}
};

console.log(person.__proto__ === Object.prototype); //true
console.log(Object.prototype.constructor === Object); //true
/*
Object.prototype은 함수의 portotype이라는 속성으로
Object()생성자 함수를 통해 생성될 객체의 프로토타입 객체를 칭하고
이 생성될 
*/
```

