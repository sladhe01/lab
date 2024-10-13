가드에 사용되는 canActivate 함수의 인자로 ExecutionContext의 인스턴스가 사용된다.
이 ExecutionContext는 ArgumentsHost 클래스를 상속받는데 요청과 응답에 대한 정보를 포함하고 있다.

ExecutionContext 인스턴스의 구체적인 모습을 보면 다음과 같이 args라는 배열을 담고 있는 객체이다.
```
//ExcutionContext 객체
{args:[]}
```
args[0]가 가르키는 것은 gql이 아닌 http 통신일 때 request 정보를 담은 IncomingMessage 객체다.
args[1]은 gql에서는 리졸버의 인자로 사용된 매개변수들을 나타낸다.
args[2]는 GraphQL 또는 REST API 요청의 상태와 관련된 정보를 저장하는 context객체를 나타낸다. gql 한정해서 보자면 통신 프로토콜에 따라 다음과 같은 형태의 객체를 가리킨다.
```
//http 프로토콜 이용시 context 객체
//IncomingMessage 객체를 나타낸다.
//토큰 등은 이 객체의 rawHeaders 에서 찾을 수 있다.
{req:{IncomingMessage 인스턴스}}


//ws 프로토콜 이용시 context 객체
//ws 프로토콜 이용시 header등을 이용하지 못하기 때문에 토큰 등의 것들은 connectionParams를 이용하여 전달해야한다.
{  req: {
    connectionInitReceived: true,
    acknowledged: true,
    subscriptions: { '30fc864c-8ddb-40e0-aa6a-2d065bbe60fe': null },
    extra: { socket: [WebSocket], request: [IncomingMessage] },
    connectionParams: {}
  }}
```

## GraphQLModule.forRoot\<ApolloDriverConfig>의 옵션
아래와 같은 모듈 설정에서 context와 관련된 context와 onConnect에 대해서 알아보자
```
GraphQLModule.forRoot<ApolloDriverConfig>({
	driver: ApolloDriver,
	autoSchemaFile: true,
	context: async () => {},
	subscriptions: {
		'graphql-ws': {
			onConnect: () => {},
		},
	},
}),
```

- graphql-ws 라이브러리에 서버를 시작할 때 context함수의 인자를 args[2]를 그대로 가져오지 않고 ctx라는 객체로 초기화 하여 건네준다. 그래서 기존 `{req:}`형식의 객체가 아니라 req의 값에 해당하는 객체 형태이기 때문에 context에서 req라는 속성의 유무로 http 프로토콜인지 ws 프로토콜인지 분간할 수 있게된다.
```js
//node_modules/graphql-ws/lib/server.js의 일부
function makeServer(options) {
...
	const ctx = {
		connectionInitReceived: false,
		acknowledged: false,
		subscriptions: {},
		extra,
	};
```
- context는 인자로 context객체를 받는다. 그리고 반환값을 context객체에 추가하도록 되어있다.
- onConnect도 인자로 context객체를 받고 함수를 그대로 실행한다. 다만 웹소켓 연결이 되면 가장 먼저 호출된다.