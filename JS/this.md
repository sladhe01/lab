
### 기본적으로 js에서 this는 global context(window)를 지칭한다.


```
	console.log(this); // Window { }
	//함수의 경우
	function a1() { console.log(this); }
	a1(); // Window{} 단, strict 모드에서는 undefined
```

### 객체의 메서드인 경우는 객체를 지칭한다.

```
	const obj = {
		method1: function() {console.log(this);}
	}
	obj.method1() // obj
	
	const a2 = obj.method1
	a2() // window
	//a2는 obj.method1을 꺼내온 것이기 때문에 더이상 메서드가 아니라 변수에 담긴 일반함수이기 때문
	//호출하는 함수가 객체의 메서드인지 일반 함수인지 중요하다.

```
### 클래스 내부의 this는 그 클래스 인스턴스 객체를 지칭한다.
``` js
class Person {
		constructor(name, age) {
			this.name = name;
			this.age = age
			}
		printThis() {
			console.log(this)
		}
	}

const person1 = new Person(Kim, 29)
person1.printThis()
//Person {name: "Kim", age: 29} 
//여기서 This는 Person클래스의 인스턴스 객체인인 person1 객체를 지칭
```
#### 좀 더 세부적인 예를 통해 알아 보자.
###### 일반 함수의 경우 상기 서술했던 대로 전역변수를 지칭한다.

```
	function foo() {
		console.log("foo's this: ", this); // Window
		function bar() {
			console.log("bar's this: ", this) // Window
		}
		bar();
	}
	foo();
	// foo's this: Window {}
	// bar's this: Window {}
```

##### 메서드의 내부 함수의 경우에도 this는 window를 지칭한다.

```
	const value = 1;
	
	const obj = {
		value: 100,
		foo: function () { // 메서드 내부 함수
			console.log("foo's this: ", this) // obj
			console.log("foo's this.value: ", this.value) // 100
			function bar() {
				console.log("bar's this: ", this) // Window
				console.log("bar's this.value: ", this.value) // 1
			}
			bar();
		}
	}
	
	obj.foo()
	// foo's this: obj
	// foo;s this.value: 100
	// bar's this: Window {}
	// bar's this.value: 1
```

##### 콜백 함수의 경우도 this는 window를 지칭한다.

```
	const value = 1;
	
	const obj = {
		value: 100,
		foo: function() {
			setTimeout(function() { // 콜백함수
				console.log("callback's this: ", this) // Window
				console.log("callback's this.value: ", this.value) // 1
			}
			,100 )
		}
	};
	
	obj.foo();
	//callback's this: Window {}
	//callback's this.value: 1
```

##### 메서드인 경우는 상기 서술한 대로 객체를 지칭한다.

```
let obj1 = {
	name: "Kim",
	sayName: function () {console.log(this.name)}
}

let obj2 = {
	name: "Jeong"
}

obj2.sayName = obj1.sayName
console.log(obj2.sayName) // function () {console.log(this.name)}
//obj1의 메서드를 할당하는 것이 아니라 obj1의 sayName이라는 이름의 프로퍼티가 나타내는 함수 자체를 obj2의 sayName에 할당한 것

obj1.sayName() // Kim
obj2.sayName() // Jeong

```
##### [[Prototype]] 객체의 메서드의 this도 프로토타입 객체를 지칭한다.

### binding

프로그래밍에서 value와 identifier 사이의 연관을 binding이라고 부른다. 그리고 이러한 연관을 지어주는 것도 binding이라고 한다. this도 binding을 통해서 원하는 객체로 정해줄 수 있는데 대표적인 방법으로 .bind() 메서드를 사용하면 된다. bind()메서드는 this 객체를 원하는 대로 정한 새로운 함수객체를 생성는 메서드이기 때문에 기존 함수를 호출하면 원래의 this를 볼 수 있다.
\**bind메서드로 생성된 함수객체에 다시 bind메서드를 사용해도 효과 없음*

```js
	function a1() { console.log(this); }
	a1.bind({hi:"hello"})() // {hi:"hello"}
	a1() // Window{}
```

이벤트 리스너안의 콜백함수인 이벤트 핸들러 함수의 경우도 내부에 this를 event target으로 바인딩되는 로직이 포함되어 있기 때문에 이벤트 핸들러 함수의 this는 기본적으로 event.target을 나타낸다.

### 화살표 함수

화살표 함수는 자신을 감싸고 있는 스코프에서 this를 찾는다. 즉 화살표함수는 일반함수와 달리 함수가 선언된 위치에 의해서 결정되고  함수가 호출된 방법과는 무관하다.