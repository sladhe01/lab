<br/>

### prototype object
프로토타입 기반 객체지향 언어인 JS는 클래스 없이도 객체를 생성할 수 있다.(Java와 같은 클래스 기반 객체지향 언어는 클래스가 있어야 한다.)
JS의 모든 객체는 부모 역할을 하는 객체와 연결되어 있고 부모 객체의 프로퍼티, 메서드를 상속받아 사용가능한데 이러한 부모 역할을 하는 객체를 Prototype 객체라고 부른다.
다음은 ECMAScript의 공식문서에 나와있는 내용이다.
- JS의 모든 객체는`[[ProtoType]]`이라는 internal slot을 가진다.
- `[[ProtoType]]`의 값은 null 또는 객체이며 상속을 구현하는데 사용된다.
- `[[ProtoType]]`객체의 데이터 프로퍼티는 get 액세스를 위해 상속되어 자식 객체의 프로퍼티처럼 사용될 수 있다. 하지만 set 액세스는 허용되지 않는다.
`[[Prototype]]`이 null이 아니라면 프로토타입 객체이고 `__proto__` 속성을 이용하여 접근할 수 있다.
`__proto__` 속성으로 접근하면 내부적으로 **Object.getPrototypeOf** 가 호출되어 프로토타입 객체를 반환한다.
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
자바스크립트는 특정 객체의 프로퍼티나 메서드에 접근할 때 해당 객체에 그 프로퍼티나 메서드가 없다면 `[[Prototype]]`이 가리키는 링크를 따라 자신의 부모역할을 하는 프로토타입 객체의 프로퍼티나 메서드를 차례대로 검색하는데 이것을 프로토타입 체인이라고 한다.
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
1. 객체 리터럴을 이용하여 생성할 때(Object( )생성자를 이용하여 생성할 때)
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
Object.prototype은 Object함수를 통해 생성될 객체의 프로토타입 객체를 칭하고
이렇게 생성될 함수의 생성자 함수는 Object 함수이기 떄문에
constructor 프로퍼티는 Object 함수를 칭한다.
*/
console.log(Object.__proto__ === Function.prototype); //true
//Object도 함수이기 떄문에 이 함수의 프로토타입 객체는 Fucntion.prototype을 칭한다.
console.log(Function.prototype.__proto__ === Object.prototype); //true
//Object가 Function 즉 함수를 통해서 생성된 객체이기 때문이다.
```
즉, 객체 리터럴을 이용하여 생성된 객체의 프로토타입 객체는 Object.prototype이다.

2. 생성자 함수를 이용하여 생성할 때
	생성자 함수로 객체를 생성하기 전에 생성자 함수를 먼저 정의해야하고 정의하는 데 3가지 방식이 있다.
	1.  함수 선언식을 이용하여 정의하는 방법
	2.  함수 표현식을 이용하여 정의하는 방법
	3.  Function( )생성자 함수를 이용하여 정의하는 방법
```js
//함수 선언식을 이용하여 정의하는 방법
let f1 = function f1(number) {
	 return number
}
//함수 선언식을 이용할 경우 자바스크립트 내부적으로 기명 함수 표현식으로 변환한다.
```

```js 
//함수 표현식을 이용하여 정의하는 방법(함수 리터럴 방식을 이용)
let f2 = function(number) {
	return number
}
```
결국 함수 선언식, 함수 표현식을 이용하는 방식 모두 함수 리터럴 방식을 이용하고 함수 리터럴 방식은 내장함수를 통하여Function( )생성자 함수를 이용하여 생성하는 것이다. 즉, 어떤 방식을 이용하여 정의하더라도 모든 함수 객체의 프로토타입 객체는 Function.prototype이다. 생성자 함수도 함수 객체이기 때문에 생성자 함수의 프로토타입 객체는 Function.prototype이다.
```js
let Person(name, gender) {
	this.name = name;
	this.gender = gender;
	this.sayHeloo = function(){
		console.log('Hi my name is' + this.name);
	};
}

let foo = new Person('Jeong',31);

console.log(foo.__proto__ === Person.prototype); //true
console.log(Person.prototype.__proto__ === Object.prototype); //true
console.log(Person.prototype.constructor === Person); //true
console.log(Person.__proto__ === Function.prototype); //true
console.log(Function.prototype.__proto__ === Object.prototype); //true
```
위와 같이 foo 객체의 프로토타입 체인 링크를 타고 가다보면 Object.prototype 객체에서 체인이 끝난다. 이 때
Object.prototype을 프로토타입 체인의 종점(End of prototype chain)이라고 한다.

### 프로토타입 객체의 확장
프로토타입도 객체이기 때문에 프로퍼티를 추가, 삭제할 수 있다. 추가, 삭제된 프로퍼티는 즉시 프로토타입 체인에 반영된다.
```js
function Person(name) {
	this.name = name
};

let foo = new Person('Jeong');

Person.prototype.sayHello = function(){
	console.log('Hi my name is' + this.name);
};

foo.sayHello(); //Hi my name is Jeong
```

### 원시타입의 확장
js에서 원시 타입을 제외한 모든 것은 객체이다. 그런데 원시 타입인 문자열이 객체와 유사하게 동작한다.
```js
let str = 'test';
console.log(typeof str); //string
console.log(str.constructor === String); //true
console.dir(str); //test

let strObj = new String('test');
console.log(typeof strObj); //object
console.log(strObj.constructor === String); //true
console.dir(strObj); 
/* {0: "t", 1: "e", 2: "s", 3: "t", legnth:4, __proto__: String, [[PrimitiveValue]]: "test"}
*/

console.log(str.toUpperCase()); //TEST
console.log(strObj.toUpperCase()); //TEST

```
위와같이 원시 타입 문자열과 String( )생성자를 통하여 생성된 객체의 타입은 다르다. 원시 타입은 객체가 아니므로 메서드를 가질 수 없지만 프로퍼티나 메서드를 호출할 때 원시타입과 연관된 객체로 ***일시적으로*** 변환되어 프로토타입 객체를 공유하게 된다.

또한 원시타입은 객체가 아니기 때문에 프로퍼티나 메서드를 직접 추가할 수 없다.
```js
let str = 'test';

str.method = function(){
	console.log('this is method')
};

str.method(); //TypeError: str.method is not a function
```
하지만 String.prototype에 메서드를 추가하면 원시타입과 String( )생성자로 생성된 객체 모두 해당 메서드를 사용할 수 있다.
```js
let str = 'test';

String.prototype.method = function(){
	return 'this is method'
};

console.log(str.method()); //this is method
console.log('string'.method()); //this is method
```
### 프로토타입 객체의 변경
객체