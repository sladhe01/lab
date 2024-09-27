@BeforeUpdate()데커레이터가 정상작동하려면 실제로 entity 변화가 일어나야 작동한다. 그런데 단순히 update()메서드에 그래서 typerorm의 update()메서드만 사용하면 SQL 쿼리만 전달하기 때문에 변화를 감지하지 못한다고 한다. 하지만 여러 조건에 따라 실험해본 결과 아래와 같이 user object를 통째로 를 변화시키는 부분이 있어야 변화를 감지하고 정상적으로 작동한다.
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


typeorm은 save 메서드가 일어나기 전의 entity상태를 스냅샷으로 일시적으로 기억해 save가 일어난 후의 상태와 비교하고 변경사항이 있으면 @BeforeUpdate 데커리이터가 붙은 함수를 실행하고 save를 한다 (from gpt, 맞는 설명인지는 잘 모르겠다... 내생각엔 save가 변경시 변경된 부분을 return하니까 return값이 있는 경우에만 작동하ㄱ)


query: SELECT "User"."id" AS "User_id", "User"."createdAt" AS "User_createdAt", "User"."updatedAt" AS "User_updatedAt", "User"."email" AS "User_email", "User"."password" AS "User_password", "User"."role" AS "User_role", "User"."verified" AS "User_verified" FROM "user" "User" WHERE (((("User"."id" = $1)))) LIMIT 1 -- PARAMETERS: [1]
query: SELECT "User"."id" AS "User_id", "User"."createdAt" AS "User_createdAt", "User"."updatedAt" AS "User_updatedAt", "User"."email" AS "User_email", "User"."password" AS "User_password", "User"."role" AS "User_role", "User"."verified" AS "User_verified" FROM "user" "User" WHERE (((("User"."id" = $1)))) LIMIT 1 -- PARAMETERS: [1]
query: SELECT "Verification"."id" AS "Verification_id", "Verification"."createdAt" AS "Verification_createdAt", "Verification"."updatedAt" AS "Verification_updatedAt", "Verification"."code" AS "Verification_code", "Verification"."userId" AS "Verification_userId" FROM "verification" "Verification" WHERE "Verification"."id" IN ($1) -- PARAMETERS: [1]
before!
query: UPDATE "user" SET "id" = $1, "createdAt" = $2, "updatedAt" = $3, "email" = $4, "password" = $5, "role" = $6, "verified" = $7 WHERE "id" IN ($8) -- PARAMETERS: [1,"2024-09-27T07:24:42.104Z","2024-09-27T10:31:23.434Z","1121@aab.com","$2b$10$HP/z4t3CUj1Bq6FVJBkdyuSBMHy4fYiuP83k/jCj6FpaYO61EWrZm","0",0,1]

query: SELECT "User"."id" AS "User_id", "User"."createdAt" AS "User_createdAt", "User"."updatedAt" AS "User_updatedAt", "User"."email" AS "User_email", "User"."password" AS "User_password", "User"."role" AS "User_role", "User"."verified" AS "User_verified" FROM "user" "User" WHERE (((("User"."id" = $1)))) LIMIT 1 -- PARAMETERS: [1]
query: SELECT "User"."id" AS "User_id", "User"."createdAt" AS "User_createdAt", "User"."updatedAt" AS "User_updatedAt", "User"."email" AS "User_email", "User"."password" AS "User_password", "User"."role" AS "User_role", "User"."verified" AS "User_verified" FROM "user" "User" WHERE (((("User"."id" = $1)))) LIMIT 1 -- PARAMETERS: [1]
query: SELECT "Verification"."id" AS "Verification_id", "Verification"."createdAt" AS "Verification_createdAt", "Verification"."updatedAt" AS "Verification_updatedAt", "Verification"."code" AS "Verification_code", "Verification"."userId" AS "Verification_userId" FROM "verification" "Verification" WHERE "Verification"."id" IN ($1) -- PARAMETERS: [1]
query: UPDATE "user" SET "id" = $1, "password" = $2, "updatedAt" = CURRENT_TIMESTAMP WHERE "id" IN ($3) -- PARAMETERS: [1,"1334233",1]