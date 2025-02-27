모든 ECMAScript 코드는 평가(evaluate)되기 전에 **realm**과 연결되어야 한다. **Realm**은 ES6부터 도입된 개념으로 다음과 같은 요소로 구성된다.
- Intrinsic Objects(내장 객체)집합
- ECMAScript Global Environment(전역 환경)
- 상기 전역 환경 스코프 내에서 로드된 모든 ECMAScript 코드
- 기타 관련 state 및 resource
 
 **Realm**은 아래와 같은 필드를 가지는 [[Record]]로 **Realm Record**로 표현하기도 한다.

| Field Name           | Value                                       | Meaning           |
| -------------------- | ------------------------------------------- | ----------------- |
| \[\[AgentSignifier]] | 에이전트 식별자                                    | 이 realm이 소유한 에이전트 |
| \[\[Intrinsics]]     | 필드 이름이 intrinsic keys이고 그 값이 객체인 [[Record]] |                   |
