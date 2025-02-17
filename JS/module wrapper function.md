Node.js의 경우 CommonJS 모듈 시스템을 기반으로 동작하는데 이는 파일 하나하나가 독립적인 모듈이고 각 파일은 다른 파일과 격리된 실행환경을 가짐을 의미한다. 
파일을 실행할 때는 module wrapper function으로 감싸서 실행한다.
아래와 같이 사용자가 작성한 코드가 하나의 함수로 감싸진 채로 실행된다. 이 함수가 module wrapper function인 것이다.
```js
(function(exports, require, module, __filename, __dirname) {     // 여기에 사용자가 작성한 코드가 들어감
)};
```