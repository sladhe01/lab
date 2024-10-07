
## Resolver와 쿼리문의 매개변수 입력방식 차이
InputType을 쓰면 resolver의 인수로 @Args() 데커레이터에 항상 문자열로 된 매개변수를 전달해야하고 gql쿼리문을 쓸 때도 매개변수자리에 InputType의 이름을 쓰고 객체형태로 써야하는 반면 Args타입은 그냥 매개변수 자리에  항목 하나씩 쓰면 된다.
```
//InputType
mutation {
	login(input:{
		email:"user@mail.com",
        password:"pw"
        }) {
	        ok
            error
            token
            }
}

//ArgsType
mutation {
	login(
		email:"user@mail.com",
		password:"pw"
		) {
			ok
	        error
	        token
            }
}

```

## 
예를 들어 PickType과 같은 MappedType으로 