모든 ECMAScript 코드는 평가(evaluate)되기 전에 **realm**과 연결되어야 한다. **Realm**은 ES6부터 도입된 개념으로 다음과 같은 요소로 구성된다.
- Intrinsic Objects(내장 객체)집합
- ECMAScript Global Environment(전역 환경)
- 상기 전역 환경 스코프 내에서 로드된 모든 ECMAScript 코드
- 기타 관련 state 및 resource
 
 \*이 아래는 참고사항으로만 알아두면 좋을 것 같다.
 **Realm**은 아래와 같은 필드를 가지는 [[Record]]로 **Realm Record**로 표현하기도 한다.

| Field Name           | Value                                                                                                                                                      | Meaning                                   |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| \[\[AgentSignifier]] | 에이전트 식별자                                                                                                                                                   | 이 realm이 소유한 에이전트                         |
| \[\[Intrinsics]]     | 필드 이름이 intrinsic keys(Object, Array와 같은 JS 내장 객체들을 가리킬 때 사용하는 키로 "Object", "Array"를 뜻함)이고 그 값이 객체(Object 내장 객체 혹은 Array 내장 객체와 같이 객체 그 자체를 뜻함)인 [[Record]] | 이 realm에서 코드가 사용하는 intrinsic values       |
| \[\[GlobalObject]]   | (내장객체인)Object 또는 undefined                                                                                                                                 | 이 realm의 Global Object(전역객체)              |
| \[\[GlobalEnv]]      | Global Environment Record                                                                                                                                  | 이 realm의 global envirnment                |
| \[\[TemplateMap]]    | \[\[Site]]와 \[\[Array]] 필드를 가진 Record의 List(배열과 비슷한 명세용 데이터 구조)                                                                                            | 템플릿 객체를 realm 별로 정규화(canocalize) 하는 데 사용됨 |
| \[\[LoadedModules]]  | \[\[Specifier]]와 \[\[Module]](Module Record) 필드를 가진 Record의 List(배열과 비슷한 명세용 데이터 구조)                                                                      | 이 realm에서 import한 모듈의 매핑 정보               |
| \[\[HostDefined]]    | 임의 값(기본값은 undefined)                                                                                                                                       | 호스트 환경에서 추가 정보를 저장하는 용도                   |
