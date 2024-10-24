graphql의 mutation을 사용하기 위한 hook으로 첫번째 매개변수는 gql문법을 코드로 파싱한 `DocumentNode` 타입(실제로 어떤 코드인지는 볼 일이 없고 `gql`함수를 통해서 반환받는다.)을 받고 두번째 매개변수로 옵션을 받는다.
## Options
- variables - gql문에 들어갈 매개변수를 여기서 전달할 수 있다. 여기서 전달해도 되고 `useMutation()`을 통해 반환받은 Promise에 variables를 전달할수도 있다.
- onCompleted - mutation이 에러 없이 성공했을 때 호출될 콜백함수
- onError - mutation이 에러가 발생했을 경우 호출될 콜백함수