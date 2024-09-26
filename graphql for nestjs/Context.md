이전에는 graphql을 사용하기 위해 apollo-server- express 라이브러리를 같이 설치하여 아폴로의 기능을 graphql도 사용가능 했다. 지금은 @apollo/server 라이브러리를 설치하여 graphqlModule을 초기화할 때 driver로 ApolloDriver를 지정해줘 graphql에서 아폴로의 기능을 사용할 수 있다.

context에 대한 설명은 다음과 같다.
>The `context` function should be _asynchronous_ and return an **object**. This object is then accessible to your server's resolvers and plugins using the name [`contextValue`](https://www.apollographql.com/docs/apollo-server/data/context/#the-contextvalue-object).
>
>You can pass a `context` function to your integration function of choice (e.g., `expressMiddleware` or `startStandaloneServer`).
>
>Your server calls the `context` function _once for every request_, enabling you to customize your `contextValue` with each request's details (such as HTTP headers)

간단하게 말하면 resolver나 plugin에서 request에 담긴 것을 이용하기 위해서 존재하는 함수가 context함수이고 req에서 원하는 속성을 context 객체(contextValue)에 담아서 resolver나 plugin에서 사용할 수 있도록 반환한다. 
nestjs에서는 손쉽게 module설정과 decorator를 이용하여 사용이 가능한 방법이 존재한다.

```ts
//app.module.ts
...
GraphQLModule.forRoot<ApolloDriverConfig>({
	driver: ApolloDriver,
	autoSchemaFile: true,
	context: async ({ req }) => {
		return { user: req['user'] };
	},
}),
...
```

```ts
// resolver.ts
@Resolver()
export class UsersResolver {
...
	@Query(() => User)
	me(@Context() context) {
		console.log(context);
	}
}
```
@Context()와 같이 전체 req객체를 모두 담을 수도 있고 @Context('myProperty')와 같이 특정 속성만 가져와 context매개변수에 담을 수도 있다.