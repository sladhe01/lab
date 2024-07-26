
  이전에 Node.js 와 express를 이용해서 로그인 기능을 구현해보려 시도하는 도중에 데이터베이스에서 저장된 데이터를 읽어오는 과정에서 내가 생각한 것과 달리 실행순서가 엉켜 애를 먹었고 이에 관하여 메모 해놓았던 것을 정리해보겠다. 이 때 자바스크립트의 구동 순서와 방식에 대해서 알아보던 중 실마리가 될만한 단서를 찾았는데 이것이 바로 호이스팅이다. 
  
   호이스팅이란 자바스크립트에서 실행전에 변수나 함수의 선언을 미리 처리하는 메커니즘을 뜻한다. 그러나 어떻게 무엇을 선언하느냐에 따라서 조금의 차이점이 존재한다. 
   
   - 함수의 경우 선언이 곧 함수를 정의 하는 행위이므로 함수의 선언은 호이스팅되어 코드의 순서와 없이 함수 선언보다 먼저 호출이 가능하다.

```js
f(); // hello world
function f () {console.log('hello world')}
```

  
   - 변수의 경우 어떤 명령어로 선언하느냐에 따라 호이스팅 방식이 달라진다. 먼저 var의 경우 호이스팅을 하면 변수가 선언되며 초기화까지 이뤄진다. 여기서 초기화란 'undefined'값을 선언해줬다고 생각하면 쉽다.

```js
console.log(x) // undefined
var x = 'hello world'
console.log(x) // hello world
```

  
   - const나 let의 경우는 호이스팅을 통해서 선언은 되지만 초기화가 되지않는다. 초기화는 실제 변수의 위치에 도달했을 때 일어나기 떄문에 그전에 호출하게 되면 referenc error가 발생한다.

```js
console.log(x) // ReferenceError: Cannot access 'x' before initialization
let x = 'hello world'
console.log(x) // hello world
```

  
   - class의 경우도 let과 const와 비슷하게 클래스는 선언은 호이스팅되지만 body부분은 호이스팅되지않는다.

```js
let claaaas = new c (); 
console.log(claaaas.z) // ReferenceError

class c {
	z () {
		return 'hello world';
    	}
	}
    
let klass = new c ();
console.log(klass.z()) // hello world
```