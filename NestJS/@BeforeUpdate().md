@BeforeUpdate()데커레이터가 정상작동하려면 실제로 javascript(typescript)코드에서 entity를 변경하는 로직이 발견되어야 작동한다. 그래서 typerorm의 update(), save()메서드만 사용하면 SQL 쿼리만 전달하기 때문에 변화를 감지하지 못하고 아래의 조건문과 같이 직접 entity를 변화시키는 부분이 있어야 변화를 감지하고 정상적으로 작동한다.
```ts
async editProfile(userId: number, { email, password }: EditProfileInput) {
	const user = await this.users.findOneByOrFail([{ id: userId }]);
	if (email) {
		user.email = email;
	}
	if (password) {
		user.password = password;
	}
	return await this.users.update(userId, user);
}
```