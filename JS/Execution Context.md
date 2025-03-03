Execution context(실행 컨텍스트)는 JS엔진이 코드를 평가하고 실행하는 과정을 추적하기 위해 사용하는 논리적인 도구를 말한다. 
보통 추상적 개념이라고 표현하는데 그 이유는 각 엔진마다 실행컨텍스트를 구성하는 내부 구조와 구현 방식은 다를 수 있지만 동일한 역할을 하는 하나의 개념을 뜻하기 때문이다. 
(실제로 자바스크립트 엔진은 실행컨텍스트를 물리적인 메모리상에 구조화된 데이터 형태로 관리하지만 이 구조화 방법과 이 실행컨텍스트를 가지고 소스코드를 구현하는 방식이 다르다는 의미이다.)
##### 실행스크립트 구성요소(State component)
실행 컨텍스트는 코드 실행의 진행 상태를 추적하는 데 필요한 모든 implmentation specific state를 가지고 있다. 그리고 모든 실행컨텍스트는 아래 표에 있는 state component를 가지고 있다. 
\* 구성요소라는 일반적인 명칭이 아니라 state(상태)라는 이름이 붙은 이유는 변할 수 있는 요소의 특정 시점에 가지고 있는 값이나 정보를 의미하기 때문이다.
\* 서술하는 내용은 ECMAScript2024를 기반으로 작성되었으며 버전에 따라 내용이 상이할 수 있다.

| component            | purpose                                                                                                                                                                                                                 |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code evaluation state | 실행컨텍스트와 관련된 코드 평가(evaluation)를 perform(수행), suspend(중단), resume(재개)하는 데 필요한 모든 상태를 포함한다. 즉 코드가 어디까지 실행되었는지, 현재 어떤 작업을 하고 있는지 등의 정보를 담고있다.                                                                               |
| Function              | 실행 컨텍스트가 함수 객체(function object)의 코드를 평가할 때 이 구성요소의 값은 해당 함수 객체가 된다. 스크립트나 모듈의 코드를 평가할 때 이 값은 `null`이 된다.                                                                                                                |
| [[Realm]]             | 코드가 ECMAScript 리소스에 접근하는 Realm Record를 나타낸다.                                                                                                                                                                            |
| ScriptOrModule        | 코드가 기원한 Module Record 혹은 Script Record를 나타낸다. 스크립트(import나 export 없이 작성된 일반적인 JS 파일로 생각하면 됨)나 모듈(export를 통해서 다른 파일에서 사용 가능한 코드 단위라고 생각하면 됨)에서 기원하지 않은 경우(InitiailizeHostDefinedRealm에서 생성된 실행컨텍스트의 경우) 이 값은 `null`이 된다. |
\*실행컨텍스트에 의한 코드 평가는 도중에 일시 중단될 수 있다. 코드 평가 중 특정 지점에서 한 **running execution context**(현시점 실제 코드를 실행시키는 실행컨텍스트, EC1이라고 표현하겠음)가 일시중단 되면 다른 실행컨텍스트(EC2)가 **running execution context**가 되고 EC2와 관련된 코드를 실행시킨다. 후에 EC1이 다시 running execution context가 되면 중단된 지점부터 EC1과 관련된 코드가 실행을 이어간다. 보통 running execution context의 전환은 스택 처럼 LIFO(last-in/first-out)방식으로 이루어지지만 ECMAScript의 일부 기능은 non-LIFO 방식으로 전환이 이뤄지기도 한다.
\*Running execution context의 realm 값을 **current Realm Record**, Function component의 값을 **active function object** 라고 칭한다.

##### 실행컨텍스트 추가 구성요소(additional state components)

| component          | purpose                                                                                  |
| ------------------- | ---------------------------------------------------------------------------------------- |
| LexicalEnvironment  | 식별자 참조를 결정한다. 즉, 코드에서 사용되는 변수나 함수 등이 정의되는 [[Environment Records]]                        |
| VariableEnvrionment | var 키워드를 사용해 선언된 변수 등이 정의되는 [[Environment Records]]                                      |
| PrivateEnvironment  | class 내에서 사용되는 Private Name에 대한 정보를 담은 [[Environment Records]]. 만약 클래스가 없다면 null 값을 가진다. |

##### 종류
실행컨텍스트를 생성시키는 소스코드의 종류에 따라 실행 컨텍스트의 생성과정과 관리하는 내용이 다르다.

- global code : **Script**로 처리되는 소스코드다. 함수나 클래스의 선언, 표현식 등은 글로벌 코드에 포함되지 않는다. (함수등을 변수에 할당하는 부분은 글로벌 코드에 속한다.)
- 
###### 실행컨텍스트의 종류
1. 
JS의 실행 과정을 자세히 말해보면 소스코드를 통해서
컨텍스트는 JS엔진이실행컨텍스트를 구성하는 요소들은 실제 메모리에 존재하지만 실행컨텍스트 자체는 개념적인 구조다 scope, hoisting, this, function, closure 등의 동작원리도 execution context로 설명이 가능하다.

자바스크립트 엔진은 코드를 실행하기 위해 실행에 필요한 여러가지 정보를 알고 있어야 한다. 예로 아래와 같은 것들이 필요하다
2. 변수 : 전역변수, 지역변수, 매개변수, 객체의 프로퍼티
3. 함수 선언
4. 변수의 스코프
5. this
이와 같이 실행에 필요한 정보를 형상화하고 구분하기 위해 JS엔진은 실행 컨텍스트를 물리적 객체의 형태로 관리한다.