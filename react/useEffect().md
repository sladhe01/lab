	`useEffect` is a React Hook that lets you synchronize a component with an external system.
리액트 공식 사이트 설명은 위와 같지만 좀 더 직관적으로 설명하자면 컴포넌트의 렌더링 외에 발생하는 작업을 처리할 때 사용하는 훅이라 할 수 있다.

```js
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
	const [serverUrl, setServerUrl] = useState('https://localhost:1234');
	useEffect(() => {
		const connection = createConnection(serverUrl, roomId);
		connection.connect();
			return () => {
				connection.disconnect();
			};
		}, [serverUrl, roomId]);
		// ...
}
```

- setup - useEffect의 첫번째 매개변수는 Effect의 로직을 담은 콜백 함수다. 여기서 또 다른 콜백함수를 리턴하는데 이는 cleanup function이라 부르며 컴포넌트가 unmount, update(props가 바뀌기 전)에 실행되는 함수다.
- dependencies - 옵셔널한 매개변수로 언제 useEffect가 실행될 지 결정한다. 생략했을 경우 useEffect는 매 렌더링마다 실행된다. 빈 배열\[ ]을 넣으면 처음 렌더링 될 때 한번만 실행된다. 특정 값(props, state 등)을 넣으면 처음 렌더링 될 때와 그 값이 변할 때만 useEffect가 실행된다.

