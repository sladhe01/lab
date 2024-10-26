```ts
const client = useApolloClient();
...
client.writeFragment({
	/*id가 꼭 schema의 id와 동일할 필요는 없고 고유하게 식별될 수 있으면 상관없다.
	단, ApolloClient가 기본적으로 캐시에 고유값을 저장할 때 타입명과 'id'를 조합하여
	생성하기 떄문에 지정하려는 고유값은 캐시키의 규칙을 따르거나 cache 옵션으로 맞추어
	설정해야한다.
	*/
	id: userData.me.id + "",
	fragment: gql(`
		fragment VerifiedUser on User {verified}
	`),
	data: { verified: true },
});

```
캐시 안에 저장된 특정id를 대상으로 제공된 GraphQL 프래그먼트 형태의 데이터를 직접 작성(저장)할 수 있도록 해준다. id는 string타입을 받도록 되어있다.