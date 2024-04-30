'{ }'를 이용하여 객체를 만들 수 있다.
하지만 유사한 객체를 여러 개 만들어야 할 때는 'new' 연산자와  생성자 함수를 사용하면 편리하다.

### 생성자 함수
<br/>
생성자 함수도 일반적인 함수와 다를 것이 없지만 관례상 아래의 두 가지 규칙을 따른다.
- 함수 이름의 첫 글자로 대문자를 사용한다.
- 'new' 연산자를 붙여 호출한다.

```
function User(name) {
	this.name = name;
	this.isAdmin = false;
}

let user = new User("Jeong");

console.log(user.name);// Jeong
console.log(user.isAdmin);// false
```

new 연산자를 통해 함수를 호출하면 다음과 같은 알고리즘을 통해  새로운 객체가 생성된다.
1. 빈 객체를 만들어 this에 할당한다.
2. 함수 본문을 호출하여 this에 새로운 프로퍼티를 추가하여 this를 수정한다.
3. this를 반환한다.

*new 연산자를 붙여 실행하면 어떤 함수라도 위와 같은 알고리즘을 거쳐 생성자 함수로 역할을 할 수 있다.*

### 생성자와 return문

생성자 함수엔 보통 retrun문이 없다. new 연산자를 이용하여 호출 시 자동으로 this를 반환한다.
하지만 생성자 함수에 return문을 추가하면
- 객체를 반환하면 this 대신 그 객체가 반환된다.
- 원시형을 반환하면 return은 무시되고 원래대로 this가 반환된다.