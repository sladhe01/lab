아폴로 클라이언트의 캐시에 대해 간단한 메모들

# Query와 cache
첫번째 Query 타입의 요청을 하면 백엔드로 요청을 보내고 그 결과를 로컬 캐시에 저장한다. 이후 동일한 쿼리를 다시 요청하면, 아폴로 클라이언트는 로컬캐시에서 데이터를 검색하고 반환한다.
\*반복요청 도중 결과값이 바뀔 것을 생각한다면 cache를 일정시간마다 refresh 시키는 방법을 사용하면 된다.

# 고유식별값
ApolloClient가 기본적으로 캐시에 고유값을 저장할 때 타입명과 'id'를 조합하여 생성하기 떄문에 id가 아닌 다른 필드로 지정하려는 고유값은 캐시키의 규칙을 따르거나 cache 옵션으로 맞추어 설정해야한다.

# InMemoryCache
## TypePolicies
### Query
GraphQL의 Query 요청 타입에 대한 정책뿐만 아니라 클라이언트 캐시 내에서만 존재하는 필드에 대한 내용을 함께 정의할 수 있다. 이런 방식으로 캐시를 이용하는 로컬변수를 사용할 수 있다.
```ts
const token = localStorage.getItem(LOCALSTORAGE_TOKEN);
export const isLoggedInVar = makeVar(Boolean(token));

const client = new ApolloClient({ 
	link: splitLink,
	cache: new InMemoryCache({
		typePolicies: {
			Query: {
				fields: {
				//캐시내에만 존재하는 타입이지만 Query타입 정책 부분에 함께 써준다.
					isLoggedIn: {
						read() {
							return isLoggedInVar();
						},
					},
...
```

