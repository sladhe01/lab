모든 ECMAScript 코드는 평가(evaluate)되기 전에 **realm**과 연결되어야 한다. **Realm**은 ES6부터 도입된 개념으로 다음과 같은 요소로 구성된다.
- Intrinsic Objects(내장 객체)집합
- ECMAScript Global Environment(전역 환경)
- 상기 전역 환경 스코프 내에서 로드된 모든 ECMAScript 코드
- 기타 관련 state 및 resource
 
 **Realm**은 **Realm Record**로 표현하기도 하며 아래 표와 같은 필드를 가진다.
 