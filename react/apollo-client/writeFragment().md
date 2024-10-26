```ts
const client = useApolloClient();
...
client.writeFragment({
	id: `User:${userData.me.id}`,
	fragment: gql(`
		fragment VerifiedUser on User {verified}
	`),
	data: { verified: true },
});

```
캐시 안에 저장된 특정id를 대상으로 제공된 GraphQL 프래그먼트 형태의 데이터를 직접 작성(저장)할 수 있도록 해준다. apollo client의 기본적인 id 식별방법에 따르면 id는 타입명 + id를 조합한 string값을 받도록 되어있다.