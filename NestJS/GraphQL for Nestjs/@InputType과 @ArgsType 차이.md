
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

## 단항목과 다항목
예를 들어 PickType과 같은 MappedType으로 뽑은 항목이 낱개의 필드항목이 아니라 여러 필드를 포함한 클래스와 같은 항목을 뽑기위해서는 그 뽑히는 항목은 ArgsType일 수 없다.
예를 들어 다음과 같은 dto를 나타내는 ArgsType이 있다고 보자.
```
//create-order.dto.ts
@ArgsType()
export class CreateOrderInput extends PickType(
	Order,
	['dishes', 'restaurant', 'total'],
	ArgsType,
) {}
```
여기서 dishes는 Dish라는 여러 필드를 포함하고 있는 타입이기 때문에 dishes는 Args타입일 수 없다.

그래서 엔터티를 나타내는 클래스의 데코레이터로 InputType을 붙여주고 MappedTypes의 마지막 매개변수로 ArgsType을 선택해 쿼리를 쓸 때 좀 더 간단한 형식으로 쓰는 방식을 선호한다.
