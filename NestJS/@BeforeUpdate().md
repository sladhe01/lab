@BeforeUpdate()데커레이터가 정상작동하려면 실제로 entity 변화가 일어나야 작동한다. 그런데 단순히 update()메서드에 그래서 typerorm의 update(), save()메서드만 사용하면 SQL 쿼리만 전달하기 때문에 변화를 감지하지 못하고 아래의 조건문과 같이 직접 entity를 변화시키는 부분이 있어야 변화를 감지하고 정상적으로 작동한다.
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


typeorm은 save 메서드가 일어나기 전의 entity상태를 스냅샷으로 일시적으로 기억해 save가 일어난 후의 상태와 비교하고 변경사항이 있으면 @BeforeUpdate 데커리이터가 붙은 함수를 실행하고 save를 한다 (from gpt, 맞는 설명인지는 잘 모르겠다... 내생각엔 save가 변경시 변경된 부분을 return하니까 return값이 있는 경우에만 작동하ㄱ)