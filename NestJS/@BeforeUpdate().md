@BeforeUpdate()데커레이터가 정상작동하려면 실제로 entity 변화가 일어나야 작동한다. 그런데 단순히 update()메서드에 그래서 typerorm의 update()메서드만 사용하면 SQL 쿼리만 전달하기 때문에 변화를 감지하지 못한다고 한다. 하지만 여러 조건에 따라 실험해본 결과 아래 첫번째 경우와 같이 find를 통해서 조회한user 객체가 update의 변경 부분으로 들어가기 때문에 실제로 user객체에 변화가 없어도 변화했다고 인지하고 @BeforeUpdate를 포함한 함수가 호출된다. 하지만 두번째 경우는 user객체를 update의 두번째 매개변수로 사용하지 않아 @BeforeUpdate를 포함한 함수가 호출되지 않는다.
```ts
async editProfile(userId: number, { email, password }: EditProfileInput) {
	const user = await this.users.findOneByOrFail([{ id: userId }]);
	if (email) {
		user.email = email;
		const { verified } = user;
	}
	if (password) {
		user.password = password;
	}
	this.users.update(userId, user); (O)
}

async editProfile(userId: number, { email, password }: EditProfileInput) {
	const user = await this.users.findOneByOrFail([{ id: userId }]);
	this.users.update(userId, {email,password}); (X)
}
```
save 메서드의 경우는 조금 다르다. save 메서드는 entity에 변화가 있으면 변화된 부분을 return하고 없으면 null을 return한다. 그래서 return 값이 있으면 @BeforeUpdate를 포함한 함수가 호출된다.
이 차이점을 알고 update()메서드를 이용하여 @BeforeUpdate()데커레이터가 작동하지 않도록 피해갈 수도 있다.